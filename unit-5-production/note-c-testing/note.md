---
icon:
  type: flat-color-icons:inspection
order: 3
---
# Testing Tiers

*Fourteen tiers, each owning one class of failure*

---

[[toc]]

---

# Two rules hold everywhere

- **One owning tier per failure class.** If no tier owns a class of bug, it ships.
- **No tier without a negative fixture.** A check nobody has watched fail is not a check.

There are no git hooks in the mono-repo, so nothing runs at commit time. The first gate is the pull request.

# The tiers

The letters follow the mono-repo's testing runway.

| Tier | Failure class it owns | Status on main |
| --- | --- | --- |
| **A** Structure | Layer violations, package cycles, app to app imports, manifest drift, unused files and exports | Every PR |
| **B** Logic | Unit behaviour of the libraries, properties that must hold for any course or calendar, timezone dependence | Every PR; nightly timezone matrix |
| **C** Generated output | An unintended change in the course JSON or site a generator produces | PRs that touch a generator; nightly corpus |
| **D** Requirements | Gherkin features with EARS Rules, bound to product code | Every PR |
| **F** Authorisation | Who may call what | Planned |
| **G** User journeys | The whole system through a real browser, against the built images | Every PR (Chromium, WebKit); nightly (Firefox, mobile) |
| **H** Message contracts | Realtime and broadcast protocols as a versioned contract | Planned; shapes are snapshot checked today |
| **I** Data | Migrations and the move of data between stores | Planned |
| **J** Platform | Kubernetes overlays, the OpenShift restricted SCC, arbitrary UID, read-only root, environment completeness | Every PR |
| **K** Observability | Log schema, request id correlation, hook order, the metrics an alert queries | Every PR |
| **L** Performance | Client bundle ceilings, Lighthouse floors, latency, memory growth | Every PR (bundles); nightly Lighthouse and k6 load |
| **M** Security | Response headers, cookie flags, CSRF, the mutating route inventory, dependency advisories | Every PR |
| **N** Completeness | Translations, theme tokens, icons, dead links in tracked Markdown | Every PR |
| **O** Suite health | `.only`, undated skips, assertionless tests, files no runner collects, per-file time budgets | Every PR; nightly with no retries |

Alongside the lettered tiers sit contract tests (the public API of the JSR packages, Supabase row and RPC shapes, realtime message shapes, generated course JSON), mutation testing (next note) and release artifact checks on release candidates.

On `main` today Vitest runs **3,178 tests in 180 files**, up from 27 tests on 1 June 2026.

# One gate

`ci.yml` ends in a single job, **CI success**, which needs every gating job: build and test, platform conformance, container smoke (and its negative fixtures), dependency audit, the end to end journeys against built images, bundle budgets and the generator differential. A skipped or cancelled job counts as a failure. Type checking is part of it: `svelte-check` on all four apps fails the job on any type error.

# Ratchets that only move one way

## Baselines

Checks that found problems on day one hold them in a baseline text file beside the suite:

- A problem that is not in the baseline fails the build.
- A baseline line that no longer occurs also fails the build.
- Fixing something therefore means deleting its line. Baselines may only shrink.

There are baselines for dependency-cruiser violations, manifest drift, unused code, completeness gaps, suite health findings, missing security headers, timezone failures, accessibility violations per page and elements that still animate under reduced motion.

## Coverage floors

Coverage is measured over **every** source file, not only the files a test happens to import, which is why it is honest. Each floor is the measured percentage rounded down. Falling below a floor fails the pull request, and rising two points above one also fails until the floor is raised in the same change, so the floor always tracks reality.

| Scope | Statements | Branches | Functions | Lines |
| --- | ---: | ---: | ---: | ---: |
| Whole repository | 72 | 66 | 71 | 73 |
| `jsr/model` | 98 | 91 | 100 | 99 |
| `jsr/time` | 92 | 81 | 94 | 93 |
| `jsr/gen` | 76 | 74 | 82 | 76 |

The whole repository figure was 55% of statements when coverage first counted every file on 26 September 2026.

## Quarantine with an expiry

A test may be skipped without a baseline entry only if the line above it names an issue and a date. After that date the skip fails again:

~~~ts
// quarantine: #123 until 2026-10-01
it.skip("flaky in webkit", () => { ... });
~~~

# What blocks a release

| Stage | Blocks on |
| --- | --- |
| Pull request to `main` | CI success, including coverage floors, new or stale baseline lines, an unclaimed generator difference, a bundle over its ceiling, a new advisory, a failed journey |
| Push to a release candidate branch | The RC readiness gates (lint and types, unit and BDD, contract, extended fuzz, CLI, build, audit, cross-browser end to end, artifact regression), then the release harness |
| Nightly | Nothing. A red nightly is a bug to chase, not a merge block |

# Read more

- [TESTING-OVERVIEW.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/guides/testing/TESTING-OVERVIEW.md): every tier, command and baseline
- [How our testing grew](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/guides/testing/journey.md): the story and the reasons
