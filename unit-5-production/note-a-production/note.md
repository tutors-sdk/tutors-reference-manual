---
icon:
  type: flat-color-icons:approval
order: 1
---
# Production Capability

*How Tutors became an emerging production grade platform*

---

[[toc]]

---

# From teaching tool to production service

Tutors started as a fast moving open source teaching tool. On 1 June 2026 the flagship reader had no tests, no CI workflow and no Dockerfile, and across the whole ecosystem there were 27 tests in one side repository.

Today the reader, the generators and the supporting apps live in one repository, the [Tutors mono-repo](https://github.com/tutors-sdk/tutors-mono-repo), and every change to it passes through a set of checks built to answer one question: *would we notice if this were wrong?*

This unit describes that capability as it exists on `main` today. It is written for anyone who wants to judge whether Tutors can be trusted with real students at scale: a lecturer, a college IT team, or a new contributor.

# The capability at a glance

| Area | What is in place on main | Read more |
| --- | --- | --- |
| Requirements | 217 EARS Rules in 50 feature files, 425 scenarios, every Rule passing an audit that runs on every pull request | EARS Requirements note |
| Tests | About 3,200 Vitest tests in 180 files, fourteen testing tiers, coverage floors that can only rise, shrink-only baselines | Testing Tiers note |
| Mutation | Stryker mutates the core libraries every night: fourteen targeted modules held at 90%, every library module held to its own floor | Mutation Strategy note |
| Release | A separate release harness runs each candidate beside production and fails on any difference nobody claimed, then explains it on a daily A3 | Release Harness note |
| Observability | One JSON log line per event with a fixed contract, request ids end to end, Prometheus metrics, health probes, SLOs with burn-rate alerts | Observability note |
| Supply chain | Multi-arch images on quay.io, scanned with Trivy, signed with cosign, an SBOM and SLSA provenance on every image, digest-pinned Kubernetes and OpenShift overlays | Images & Deployment note |

# How the numbers grew

The growth came in steps, each answering a different question:

| When (2026) | Step | The question it answered |
| --- | --- | --- |
| July | The reader's first tests and workflow; the move to a mono-repo | Can we test this at all? |
| 6 August | A testing framework: 27 to 1,409 tests in a week | How many tests can we have? |
| 17 September | The testing runway: negative fixtures, shrink-only baselines, tiers A to O | Can these tests fail, and can the debt grow? |
| 19 to 21 September | Feature files bound to product code; EARS Rule ids and the audit gate | Do the requirements test anything? |
| 26 September | Honest coverage floors, mutation testing breaking at 90%, nightly mutation of every library module | Would a test notice if the code were wrong? |
| Late September to October | The release harness, containers on quay.io, signed images, SLOs, the log contract | Is what we ship the thing we tested, and can we see it running? |

| | 1 Jun | 6 Aug | 21 Sep | 28 Sep | 19 Oct |
| --- | ---: | ---: | ---: | ---: | ---: |
| Tests | 27 | 1,409 | 2,124 | 2,140 | **3,178** |
| Test files | 5 | 84 | 141 | 147 | **180** |
| EARS Rules | 0 | 0 | 18 | 89 | **217** |

The history up to 28 September is from [How our testing grew](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/guides/testing/journey.md) in the mono-repo. The 19 October figures were measured on `main` for this unit.

# Principles that run through all of it

- **A check nobody has watched fail is not a check.** Every repo-level check ships with a deliberately broken fixture that it must reject first.
- **Debt can only shrink.** Problems found on day one sit in baseline files; a new problem fails the build, and so does a baseline line that no longer occurs. Coverage and mutation floors only move up.
- **Every difference is claimed.** A release may change what students see, but each change must be named in the changelog or by a Rule. The release harness fails on anything else.
- **What we test is what we ship.** A release tag promotes the exact images the harness judged, by digest, with their signature and SBOM.
- **Stop the line.** The release process borrows from Lean: a failing gate stops the release, and every stop ends in a countermeasure, never "human error".

# What "emerging" means

This is a capability that is real and enforced today, and still growing. The testing guide is explicit about the tiers that are not built yet: authorisation (tier F), message contracts for realtime (tier H) and data migration (tier I) are planned. The SLO targets are indicative, set from what the college aims for rather than from months of production traffic. The harness's policy checks (image hardening, provenance, vulnerability ceiling, timing tolerance) report on every run but do not yet block a release; they become blocking at harness 2.0 after a soak of clean nights.

Being honest about those gaps is part of the capability: every one of them is written down, owned and visible on the same pages that show what passes.

# Where to start

- The mono-repo [README](https://github.com/tutors-sdk/tutors-mono-repo#readme) and its ten minute quick start
- The [daily A3](https://tutors-sdk.github.io/tutors-release-harness/a3.html): is `main` ready to release tonight?
- The short course [How the release harness works](https://tutors-sdk.github.io/tutors-release-harness/course/index.html)
- [TESTING-OVERVIEW.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/guides/testing/TESTING-OVERVIEW.md): what each testing tier owns
- [deploy/README.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/deploy/README.md): images, logs, metrics and Kubernetes
