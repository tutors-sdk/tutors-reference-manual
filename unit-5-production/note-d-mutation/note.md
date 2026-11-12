---
icon:
  type: flat-color-icons:biohazard
order: 4
---
# Mutation Strategy

*Planting bugs on purpose to prove the tests would catch them*

---

[[toc]]

---

# Why coverage is not enough

Coverage says which lines a test *executed*. It does not say whether any test would notice if the line were wrong. A file can show 100% line coverage with no meaningful assertion in it.

Mutation testing answers the stronger question. It makes small deliberate changes (a **mutant**) and checks that something fails. A mutant that nothing catches is a **survivor**, and every survivor is a gap.

Tutors uses mutation at two levels:

| Level | What is mutated | Who must catch it | Where |
| --- | --- | --- | --- |
| Code | The source of the libraries: an operator flipped, a condition removed, a return value changed | The unit, BDD and contract tests | Mono-repo, nightly, with Stryker |
| Release | A whole production image with one planted regression: a dropped header, a 500 on a route, a package added | The release harness | Release harness, weekly |

# Code mutation in the mono-repo

## Targeted modules held at 90%

Fourteen modules hold the computation where an undetected bug would silently corrupt what students and lecturers see: search, the learning object tree, course and markdown utilities, the calendar and lab analytics models, and the generator's resource and course builders. The nightly `mutation` job runs Stryker over them and **fails below a 90% aggregate score**. Each module also has its own floor, set to its measured score rounded down:

| Module | Floor |
| --- | ---: |
| `model/types/type-utils.ts` | 100 |
| `time/utils/calendar-utils.ts`, `time/utils/lab-utils.ts` | 98 |
| `model/utils/markdown-utils.ts`, `gen/templates/utils.ts` | 97 |
| `model/utils/lo-utils.ts`, `model/utils/course-utils.ts`, `time/services/base-calendar-model.ts` | 96 |
| `gen/utils/lr-utils.ts` | 95 |
| `gen/services/course-builder.ts` | 94 |
| `time/services/base-lab-model.ts` | 87 |
| `model/services/search.ts` | 84 |
| `model/services/lo-tree.ts` | 74 |
| `gen/services/resource-builder.ts` | 72 |

The lower floors are known: they sit on equivalent mutants or code that cannot be reached, and they are written down rather than hidden.

## Every library module, every night

The nightly `mutation-nightly` job mutates **every** TypeScript source file in the packages and runs the unit, BDD and contract suites against each mutant. It is the map of where tests are missing:

- **A floor per module.** 108 modules each have a floor. A module below its floor, or a new module with no floor, fails the night and is named in the job summary.
- **Improvements never fail.** A score two points above its floor is listed as a floor to raise, in a later change.
- **Deterministic.** Two consecutive runs give identical scores for every module.
- **Proven clean.** The run mutates the working tree in place, so a final step fails the night if any tracked file was left changed.

The first baseline on 26 September 2026 scored 58.5% over 95 modules. The floors on `main` now average 66% over 108 modules, and 45 of them are at 90% or above.

## Acting on a survivor

1. Open the survivor in the HTML report (the nightly job uploads it).
2. Read what the mutation changed, for example `>` to `>=`.
3. Write a test that fails with that change.
4. Re-run the module and raise its floor in the same change.

# Release mutation in the harness

A release can be broken in ways no unit test sees: a header dropped at the edge, a slow page, a package that should not be in the image. So the [release harness](https://github.com/tutors-sdk/tutors-release-harness) keeps sixteen **mutant images**, each the production reader image with one planted fault, and each with the artefact the harness must blame:

| Mutant | Planted fault | Must be caught by |
| --- | --- | --- |
| `dropped-header` | A security header removed | headers |
| `route-500` | One route answers 500 | network, dom |
| `console-error` | A browser console error | console |
| `dom-note` | Injected HTML on a page | dom |
| `missing-alt` | An image loses its alt text | axe (accessibility) |
| `slow-ssr`, `slow-ssr-mild` | Every page 400 ms or 150 ms slower | timing, timing tolerance |
| `anon-write` | Anonymous readers write learning records | persistence |
| `focus-order` | Navigator links leave the tab order | focus |
| `extra-chunk` | Every page loads one more module | asset graph |
| `replay-header` | A header dropped on note pages only | replay |
| `base-swap` | The image rebuilt on another base | image manifest |
| `added-package` | One npm package added to the image | SBOM |
| `secret-env` | A secret-looking environment variable | image hardening |
| `vulnerable-package` | A package with a critical advisory | vulnerability ceiling |
| `unsigned-build` | The same bytes, not pulled and verified | build provenance |

`pnpm harness mutants` first runs the harness against production twice (an A/A, so it has the right to fail anything), then against each mutant. It passes only when every mutant is caught and blamed on the right artefact. It runs weekly and whenever a mask or an engine changes, and each result is recorded on the harness scoreboard by name, caught or escaped.

This is the harness testing itself: before it is trusted to say a release is safe, it has to show it can see sixteen kinds of unsafe.

# Mutants as a countermeasure

When a release stops or something escapes to production, the release process opens a 5 Whys that must end in a countermeasure. A new mutant is one of the seven allowed kinds: if the harness missed a class of fault once, a mutant makes sure it cannot miss it again.

# Read more

- [MUTATION-TESTING.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/guides/testing/MUTATION-TESTING.md) in the mono-repo
- [mutants/README.md](https://github.com/tutors-sdk/tutors-release-harness/blob/main/mutants/README.md) in the release harness
