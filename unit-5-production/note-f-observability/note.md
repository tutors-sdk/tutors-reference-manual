---
icon:
  type: flat-color-icons:line-chart
order: 6
---
# Observability

*Logs, metrics, probes and service level objectives*

---

[[toc]]

---

# What a running Tutors app tells you

All four apps (reader, catalogue, live and time) share one observability surface, built from the same packages (`@tutors/logger`, `@tutors/metrics`) and pinned by the observability tier (K) of the test suite:

| Signal | Where | Purpose |
| --- | --- | --- |
| Logs | stdout and stderr, one JSON object per line | What happened, to which request, in which pod |
| Request ids | `x-request-id` header and the `requestId` log field | Follow one request across apps and into Supabase |
| Metrics | `GET /metrics`, Prometheus text format | Request rate, errors and latency per route |
| Liveness | `GET /healthz/live` | Is the process up? No dependencies |
| Readiness | `GET /healthz` | Can it serve? Also reports Supabase reachability |
| Build identity | `GET /version` | Which app, version, commit and build date is this? |

# Logs

## One JSON line per event

In a container every line on stdout and stderr is one JSON object, from the first line to the last. Each app's server hooks route through the logger whatever would otherwise be plain text: `console` calls inside dependencies, uncaught exceptions and unhandled rejections (logged with their stack, then exit 1), and Node process warnings. A multi-line stack is one string inside its line. Development builds keep a readable `[time] [app:level] message` format.

## The log contract

Every line starts with the same keys, in the same order, always present:

| Key | Notes |
| --- | --- |
| `timestamp` | ISO 8601 |
| `level` | `debug`, `info`, `warn` or `error` (set with `LOG_LEVEL`) |
| `event` | the kind of line, for example `request.completed` |
| `message` | fixed text for the event |
| `app` | `tutors-reader`, `tutors-catalogue`, `tutors-live` or `tutors-time` |
| `environment` | `production` or `development` |
| `hostname` | the pod or container name |
| `pid` | process id |
| `requestId` | the request this line belongs to, or null |

Then the fields of the event, again all present and in order. For example a completed request:

~~~json
{"timestamp":"2026-10-19T09:05:00.000Z","level":"info","event":"request.completed","message":"request completed","app":"tutors-reader","environment":"production","hostname":"reader-7c9f6d-x2k4p","pid":1,"requestId":"5f0c...","method":"GET","path":"/course/reference-course","route":"/(course-reader)/course/[courseid]","status":200,"duration_ms":84,"slow":false,"loadError":false}
~~~

The other events are `service.start`, `request.error`, `request.failed`, `console`, `process.uncaughtException`, `process.unhandledRejection`, `process.warning` and `log`. A request taking two seconds or more is marked `slow` and raised to `warn`.

## Messages are fixed text

A log collector, and the release harness, group and compare lines by `message`. So anything that varies goes in a field, never into the message:

~~~ts
log.error("Error fetching course", { courseId, url });   // yes
log.error(`Error fetching ${url}`);                        // no
~~~

A contract test fails on any log call in product code whose first argument is not a string literal, and `pnpm check:container` checks the whole contract against real container output.

## Request ids

The request logger runs first in every app. It takes an incoming `x-request-id` when it is well formed (an ingress already traced the request) or generates a UUID, sets it once on the response, and runs the rest of the request inside an async context. Every line logged while serving the request carries the id, even from packages that know nothing about requests, and server-side Supabase calls forward it.

# Metrics

Every app serves `GET /metrics`:

- `http_request_duration_seconds` (a histogram with fixed buckets from 10 ms to 10 s), `http_requests_total` and `http_requests_in_flight`, labelled by method, SvelteKit route id and status code
- the default Node.js process metrics (CPU, memory, event loop lag, garbage collection)

No label carries a per-request or per-process value, so series stay stable between restarts. The series exist from the first scrape, so an idle app reads as idle rather than uninstrumented. In production `METRICS_TOKEN` puts the endpoint behind a bearer token.

On OpenShift, user-workload monitoring scrapes the apps through a ServiceMonitor component added to an overlay. For local and staging use, `observability/compose.yaml` starts Prometheus and Grafana already provisioned with the data source, the alert rules and contact points.

# Service level objectives

The objectives are written once, in [`release/slo.yaml`](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/release/slo.yaml), over a rolling 28 day window. The reader, which is what students read, is the strictest.

| Objective | Target | Error budget in 28 days |
| --- | --- | --- |
| Reader availability | 99.5% of responses not 5xx | about 3h 22m of total failure |
| Reader latency | 95% under 1 s | 5% of requests slower |
| Reader latency tail | 99% under 2.5 s | 1% of requests slower |
| Catalogue, live, time availability | 99% not 5xx | about 6h 43m each |
| Catalogue, live, time latency | 95% under 1 s | 5% of requests slower |

The targets are indicative: they are what the college aims for, and they will move as real traffic says what is achievable.

## Burn-rate alerts

The Grafana alert rules are generated from the same file (`pnpm slo:alerts`), and `pnpm check:slo` fails if they drift. Each objective has three multi-window burn-rate alerts for its own app, so a broken reader cannot hide behind three healthy apps:

| Alert | Fires when the budget burns at | Over | Severity |
| --- | --- | --- | --- |
| page-fast | 14.4 × | 1 h and 5 m | page |
| page-slow | 6 × | 6 h and 30 m | page |
| ticket | 1 × | 3 d and 6 h | ticket, never pages |

A separate alert covers an app whose health check is failing altogether.

## Error-budget policy

- **Over half the budget left:** release as normal.
- **More than half spent:** only low risk releases; anything touching a hot path waits or ships behind a flag.
- **Spent:** freeze feature releases for that app until the window recovers.

The release harness reads the same file and judges each candidate against it, together with its own stricter floors, so lowering a production target never loosens a release.

# Read more

- [deploy/README.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/deploy/README.md): the log contract, metrics contract and image contract in full
- [docs/slo.md](https://github.com/tutors-sdk/tutors-mono-repo/blob/main/docs/slo.md): the objectives, alerts and policy
