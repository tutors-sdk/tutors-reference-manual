---
icon:
  type: flat-color-icons:document
order: 2
---
# EARS Requirements

*Requirements that are written once, numbered, and proved by tests*

---

[[toc]]

---

# Why requirements come first

A test suite can be large and still not say what the system is meant to do. Tutors writes its requirements in **EARS** (the Easy Approach to Requirements Syntax), a small set of sentence patterns that remove ambiguity, and binds each requirement to the tests that prove it. A requirement with no proof, or a test that cites a requirement that does not exist, fails the build.

On `main` today there are **217 Rules in 50 feature files, proved by 425 scenarios**, and all 217 pass the audit.

# The five patterns

| Pattern | Shape | Tag | Rules on main |
| --- | --- | --- | ---: |
| Ubiquitous | The system SHALL ... | `@ears-ubiquitous` | 38 |
| Event-driven | WHEN [event] the system SHALL ... | `@ears-event-driven` | 109 |
| State-driven | WHILE [state] the system SHALL ... | `@ears-state-driven` | 24 |
| Unwanted | IF [condition] THEN the system SHALL ... | `@ears-unwanted` | 37 |
| Optional | WHERE [feature is enabled] the system SHALL ... | `@ears-optional` | 9 |

# Anatomy of a Rule

Each requirement is a Gherkin `Rule:` block with a four digit id, one EARS pattern tag, and exactly one "shall". The scenarios under it are its proof. This one is taken from the service level objectives feature on `main`:

~~~gherkin
@rule-0296 @ears-unwanted
Rule: If an objective in release/slo.yaml is one the release harness cannot judge, then tutors shall reject the file and name the objective.

  Scenario: A latency threshold between bucket bounds is rejected
    Given an SLO file with a "latency" objective "reader-latency" for "reader" with target 0.95 and threshold 0.75
    When the maintainer checks the SLO file
    Then the check shall reject the SLO file naming "objective 1 (reader-latency)"
    And the rejection shall also name "bucket bound"
~~~

The steps file that loads this feature calls the real product code. Steps never assert on a fixture, and the run fails when a scenario or a step exists on one side only.

Behaviour that needs a browser (layout, focus order, a dialog) is a `@ui` Rule instead. Its scenarios are proved by Playwright tests that carry the Rule id as a tag, and they run on every pull request in Chromium and on release candidates in Chromium, Firefox and WebKit.

# The audit

`pnpm test:ears:audit` runs in the `ears-audit` job of every CI run. It checks, among other things, that:

- every Rule has one id, one pattern tag and one "shall", worded with Gherkin keywords so no precondition is silently dropped
- every Rule id is unique, and retired ids are never reused
- every `@ui` scenario has a Playwright test with its exact title and Rule id, and no proving test is skipped
- no Playwright test cites a Rule it does not belong to

Today's run on `main`:

~~~text
OK: 50 feature file(s), 217 Rule(s), 0 new violation(s), 15 baselined, 0 stale baseline entries.
~~~

The fifteen baselined findings are older wording problems held in a shrink-only baseline file: they cannot grow, and fixing one means deleting its line.

# The EARS dashboard

`pnpm ears:dashboard` builds one self-contained page from the audit's own parser, so it always says what CI says:

- **Overview**: feature files, Rules and scenarios, Rules by pattern and by area, the next free Rule id, and anything that needs attention
- **Rules**: every Rule as written, with links to its source on GitHub and to the tests that prove it
- **Write a Rule**: the anatomy of a Rule, a passing example of each pattern from the repository, and the words the audit rejects

CI publishes the dashboard as the `ears-dashboard` artifact of every run. On a pull request the job summary lists the Rules the change added, changed or removed.

# Where Rules reach production

Rules are not only for tests. A release candidate's intended differences are written as claims in `release/claims.yaml`, and a claim names the Rule behind the change:

~~~yaml
claims:
  - artefact: dom
    scope: "reader:lab-step*"
    reason: "Rule 0031: lab steps shall show estimated reading time"
~~~

`pnpm check:release-claims` fails on a Rule id no feature defines. So the chain runs unbroken from a requirement, to the test that proves it, to the release that ships it (see the Release Harness note).

# Read more

- [EARS-METHODOLOGY.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/guides/testing/EARS-METHODOLOGY.md) in the mono-repo
- The feature files in [tests/bdd/features](https://github.com/tutors-sdk/tutors-mono-repo/tree/main/tests/bdd/features)
