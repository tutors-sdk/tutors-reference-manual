---
icon:
  type: flat-color-icons:scatter-plot
order: 5
---
# Release Harness

*Every observable difference between a candidate and production must be claimed*

---

[[toc]]

---

# The idea

Tests check that the code does what it should. They do not check that the *release* changes only what somebody intended. The [Tutors release harness](https://github.com/tutors-sdk/tutors-release-harness) does that:

> Run the same scripted traffic through two identical stacks that differ only in the application image, normalise the noise, diff everything observable, and fail unless each difference is claimed by a changelog entry or a Rule.

Its job is not to prove a candidate is identical to production (a release should differ) but that **every difference is claimed and nothing else moved**.

The harness is a separate project on purpose. It consumes the mono-repo's published, signed images from quay.io by tag or digest, verifies each signature, and knows nothing about their source. So it can compare any two tags, and the pull request it is judging cannot quietly weaken it.

# A/A, then A/B

- **A/A (noise mode).** Production against itself. Anything that differs here is noise: a timestamp, a request id, a random ordering. The harness learns to mask it, and a clean A/A no more than seven days old is what gives it the right to fail a release.
- **A/B (release mode).** Production against the candidate, with the same traffic, several runs per side. What remains after the noise is masked is the release's real difference.

For each page it captures the accessibility tree, a screenshot, network requests, console output, document headers, axe accessibility findings, keyboard focus order and time to first byte. Around the journeys it reads `/metrics`, container logs and every learning record written. It also reads the images themselves: manifest, SBOM, vulnerabilities, hardening and build provenance.

Other modes rehearse a **migration** (expand, contract and rollback) and an **upgrade** (a rolling update under load), and **post-deploy** replays the reference journeys against live production after a release ships.

# Claims

A contributor declares each intended difference in `release/claims.yaml` in the mono-repo:

~~~yaml
claims:
  - artefact: dom
    scope: "reader:lab-step*"
    reason: "Rule 0031: lab steps shall show estimated reading time"
~~~

A difference no claim covers fails the Gate. A claim that matches nothing is reported as stale. Reasons such as "see PR" or "approved" are rejected; a claim names a Rule or a changelog entry, and a broad claim needs a named person.

# The verdict

`harness release` always leaves a report and reads in a fixed order:

1. **The Gate**: PASS, WARN or FAIL. Every observable difference claimed, or not. The Gate alone decides the exit code.
2. **The Release Confidence Score** and its band (Green, Amber, Red). Advisory.
3. **The reviewer's glance**: at most seven ranked places for a second person to look. Each is marked verified, disputed or escalated.

# The daily A3

Every night the harness judges today's `main` against production exactly as it would judge a release candidate (**Main to RC**), and publishes an [A3](https://tutors-sdk.github.io/tutors-release-harness/a3.html), a one page report in the Lean tradition. It answers four questions:

1. What is being compared, and when?
2. Did the Gate pass, and did the harness have the right to fail?
3. How much of the gap is explained, and by what? For example: "X differences; A, B and C contributed; Y unexplained".
4. What, if anything, needs a person to look?

It counts causes, not raw diff hunks, and attributes each cause to the pull request or claim behind it.

# Control charts

Timing is never a pass or fail. It is read on **XmR control charts**, the Shewhart chart for one number per point:

- the centre line is the mean;
- the upper and lower control limits are the centre ± 2.66 × the mean moving range;
- a point outside the limits is a **special cause to investigate**: worth asking why, never a failure.

The A3 draws one chart per journey: production's limits as a band, and main's median for each night as a dot. The [overnight readiness page](https://tutors-sdk.github.io/tutors-release-harness/readiness.html) draws a second kind: the size of each past release in merged pull requests, with what is waiting on `main` now. It tells the release Captain when the batch is bigger than this project's releases usually are and it is time to cut a release.

# The release process

The mono-repo's [release SOP](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/release/SOP.md) is standard work in twelve steps, each with an owner, an input and a "done when" that is a file or a recorded action. It names three roles: the **Captain** who runs the release, a **Reviewer** who authored no pull request in it, and **Contributors** who own their changelog lines, claims and Rules.

It is built on five Lean ideas:

| Idea | Here |
| --- | --- |
| **Jidoka** (stop the line) | The Gate stops a release on any unclaimed difference. No score or argument talks it back on |
| **Visual management** | One Gate, one score, one glance, read the same way by everyone |
| **Standard work** | The twelve step SOP |
| **Gemba** (go and look) | The Reviewer goes to the artefact itself, not the summary |
| **Kaizen** | A stop or an escape opens a 5 Whys ending in one countermeasure: a mutant, a journey, a mask review, an EARS spec, claim guidance, an SOP change or a glance rule |

After the release tag, the exact images the harness judged are **promoted** (see the Images & Deployment note), the Kubernetes overlays are pinned to their digests, and post-deploy runs every fifteen minutes through a 24 hour watch window.

# Before harness 2.0

Every diff engine (DOM, network, headers, console, accessibility, focus, persistence, logs, metrics, image manifest, SBOM, vulnerabilities and the rest) is **blocking**: an unclaimed difference fails the release. Some newer checks are **informing**: they report on every run but cannot fail one yet. These are timing (apart from errors under load, which do block), image hardening, build provenance, the vulnerability ceiling, timing tolerance, the asset graph, replay, SLO and realtime. The policy checks each have a planted mutant that proves they can fire. Harness 2.0 decides which become blocking, after a soak of ten clean nights in a row, counted on the readiness page.

# Read more

- [How the release harness works](https://tutors-sdk.github.io/tutors-release-harness/course/index.html), a short Tutors course with five talks and a lab on reading the A3
- [The harness report pages](https://tutors-sdk.github.io/tutors-release-harness/): the A3, readiness, every forecast and the scoreboard
- [Running a release](https://github.com/tutors-sdk/tutors-release-harness/blob/main/docs/user-guide/10-running-a-release.md)
