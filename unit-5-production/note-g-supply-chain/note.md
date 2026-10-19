---
icon:
  type: flat-color-icons:lock
order: 7
---
# Images & Deployment

*Signed images, a software bill of materials, and OpenShift ready manifests*

---

[[toc]]

---

# One image recipe, four apps

One `Dockerfile` at the root of the mono-repo builds any of the four SvelteKit apps into a self-contained Node.js image. Runtime configuration comes from the environment, so the **same image** is promoted through every environment and no secret is baked into a layer.

| App | Image |
| --- | --- |
| reader | `quay.io/tutors-sdk/tutors-reader` |
| catalogue | `quay.io/tutors-sdk/tutors-catalogue` |
| live | `quay.io/tutors-sdk/tutors-live` |
| time | `quay.io/tutors-sdk/tutors-time` |

## The image contract

| Item | Value |
| --- | --- |
| Port | `3000` |
| User | UID `1001`, GID `0`, so it runs under OpenShift's arbitrary UIDs |
| Filesystem | Safe on a read-only root; nothing is written at runtime |
| Probes | `GET /healthz/live` (liveness), `GET /healthz` (readiness) |
| Healthcheck | Built in, against the liveness route; the image has no curl or wget |
| Metrics | `GET /metrics` |
| Build identity | `GET /version`, and OCI labels for version, revision, created and source |

Two requests to the same image answer identical headers apart from `Date` and `x-request-id`, and two builds of the same commit produce the same static file headers. That determinism is what lets the release harness compare a candidate with production without drowning in noise.

# How an image is published

The `Container Image Build` workflow builds multi-arch images (`linux/amd64` and `linux/arm64`) on every push to `main`, release candidate and release tag. For each app, in order:

1. **Scan.** Build the amd64 image and scan it with Trivy. A critical or high finding with a fix available fails the job before anything is pushed.
2. **Push by digest.** Push one multi-arch digest, with no tag yet.
3. **Sign.** Sign the digest with **cosign**, keyless: the certificate is issued to the workflow's GitHub identity, so there is no signing key to leak.
4. **SBOM.** Generate an SPDX software bill of materials with syft and attach it to the digest as a signed attestation.
5. **Provenance.** Attach SLSA v1 build provenance naming the commit built and the run that built it, and verify it before going on.
6. **Tag.** Only now point the tags at the signed digest, and fail if any tag resolves elsewhere. A tag never names an image whose signature, SBOM or provenance is still missing.

Pull requests run step 1 only, with no registry login.

Anyone can check an image:

~~~bash
cosign verify \
  --certificate-identity-regexp '^https://github.com/tutors-sdk/tutors-mono-repo/\.github/workflows/image-build\.yml@' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  quay.io/tutors-sdk/tutors-reader:<tag>
~~~

The same command with `verify-attestation --type spdxjson` returns the SBOM, and with `--type slsaprovenance1` the provenance.

# What ships is what was tested

The release harness judges the release candidate's images. So a release tag does not build new ones: it **promotes** the candidate. For each app the workflow checks that the release commit has the same git tree as the candidate, that the image's revision label is the candidate commit, and that its cosign signature verifies. Then it retags that same digest. The digest, signature, SBOM and provenance are the ones the harness judged. If an app cannot be promoted it is rebuilt, and the run says so loudly: *REBUILT: this image is not the one the release harness judged*.

# Kubernetes and OpenShift

`deploy/k8s` holds a shared base (Deployment, Service, ConfigMap, PodDisruptionBudget) and one overlay per app, with Kustomize components for an OpenShift **Route** (edge TLS, plain HTTP redirected), a Kubernetes **Ingress**, and a Prometheus **ServiceMonitor**:

~~~bash
kubectl kustomize deploy/k8s/variants/openshift/reader   # render overlay + Route
oc apply -k deploy/k8s/variants/openshift/reader          # deploy
~~~

- **Pinned by digest.** Each overlay names its image by release tag *and* digest. The runtime pulls the digest, so a tag pushed again cannot change what runs. `pnpm deploy:pin X.Y.Z` resolves each digest, verifies its signature and rewrites the four overlays in one pull request; that merge is the deploy.
- **Restricted by default.** Pods run as non-root with a read-only root filesystem, all capabilities dropped and the default seccomp profile, which satisfies OpenShift's `restricted-v2` security context constraint with no extra grants.
- **Checked on every pull request.** `pnpm check:k8s` renders every overlay and variant and enforces the policies: registry-qualified, digest-pinned images, requests and limits, probes, the security context, a Route host that matches the app's origin. CI also validates the output with kubeconform, and every variable an app reads must appear in both `.env.example` and the manifests.
- **A parallel `next` deployment.** Variants for `next.tutors.dev` and its siblings run beside production in one namespace, reusing production's pinned images.

For a single host, `deploy/staging/compose.yaml` runs the four published images with the same read-only, capability-dropped posture.

# Repository security

- **OpenSSF Scorecard** runs weekly and on pushes to `main`, and publishes to the public dashboard (badge on the README), alongside an OpenSSF Baseline badge.
- **CodeQL** analyses the JavaScript and TypeScript.
- **zizmor** audits the GitHub Actions workflows themselves.
- **Dependency audit** blocks a pull request that introduces a new advisory. Advisories that cannot affect a release are recorded as OpenVEX statements in `release/openvex.json`, which the harness's scanner reads.
- Vulnerabilities are reported privately, as described in [SECURITY.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/SECURITY.md).

# Read more

- [deploy/README.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/deploy/README.md): build, registry, verification, Kubernetes and OpenShift
- [Release-Strategy.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/guides/Release-Strategy.md): branches, versions and the path from candidate to production
