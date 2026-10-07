# Guillaume Lecomte

**CTO / Lead Developer, 14+ years.** I design, build and scale SaaS platforms and distributed systems, and I now put AI agents
to work on top of them: the platforms, integrations and guardrails that make agents reliable in production.

Based in France, fully remote. Open to **CTO, Fractional CTO or Head of AI Engineering** roles.
**[LinkedIn](https://www.linkedin.com/in/guillaumelecomtefr)** · **[guillaume-lecomte.fr](https://guillaume-lecomte.fr)**

---

## Problem > solution

| Problem I was handed | What I did | Hat |
| --- | --- | --- |
| A legacy stack that cost too much and could not be changed one piece at a time | Replaced it with modular microservices (NestJS + gRPC): **-50% infrastructure cost** | CTO |
| Data imports so slow that users waited on them | gRPC streaming: **30 min → 2-3 min (10-15× faster)** | Architect |
| A SaaS platform for a safety-critical, regulated sector, where losing data silently is not an option | Designed for traceability and humans in the loop. **Adopted by EASA and 5 European airlines** | Architect, Lead dev |
| One release process for everything, so one bad change blocked all the others | Monorepo CI/CD with **independent deployments and granular rollback** | Lead dev |
| A team to build from nothing | Hired, structured and mentored it, from first hire to CTO | Manager |
| AI agents that crash, cost too much and quietly get worse | An agent platform with durable runs, cost routing and an eval gate in CI (below) | AI engineering |

The production work is not public. Team sizes, scope, the decisions behind these results and how I work with AI agents are
detailed on **[my LinkedIn](https://www.linkedin.com/in/guillaumelecomtefr)**. The projects below are smaller, runnable
illustrations of the same habits. Each README says what works, how I checked it, and what does not work yet.

---

## Proof in public code

### [agent-platform](https://github.com/guillaume-lecomte/agent-platform): running LLM agents in production

TypeScript monorepo (NestJS, BullMQ, PostgreSQL, Redis, React) with a demo agent that triages GitHub issues. It runs locally on a
built-in mock model, with no API key.

- **An agent dies halfway through a task.** Every model call and tool result is saved as a step, so another worker resumes from the last one. Tools that change something wait for human approval and run at most once. `pnpm crash-demo` kills a worker with `SIGKILL` and checks the database. See [durable-runner](https://github.com/guillaume-lecomte/agent-platform/tree/main/packages/durable-runner).
- **Every call goes to the biggest model.** A YAML routing policy picks the model by task type and complexity, with a fallback and a Redis cache. Cost and latency are recorded per call. See [cost-router](https://github.com/guillaume-lecomte/agent-platform/tree/main/packages/cost-router).
- **A prompt change quietly makes the agent worse.** A 40-case eval suite is scored against a committed baseline, CI fails when the score drops, and a promote and rollback command moves agent versions. See [evals](https://github.com/guillaume-lecomte/agent-platform/tree/main/packages/evals).

Decisions are written as [ADRs](https://github.com/guillaume-lecomte/agent-platform/tree/main/docs/decisions). Known limits: no authentication, single Redis and PostgreSQL.

### [trip-ledger](https://github.com/guillaume-lecomte/trip-ledger): shared expenses that work offline and get time zones right

Expo mobile app, NestJS API, PostgreSQL, and a custom sync engine, in a TypeScript monorepo (4 packages, 2 apps, a simulator).

- **No network where the expenses happen, and two friends edit the same expense differently.** Every action is an immutable operation in a local journal, sent later in batches and stored once by the server; the state is a pure function of the set of operations. Different fields merge by themselves. Two different amounts become a conflict **shown to a person**, never settled by whichever phone synced last.
- **A dinner at 23:30 in Tokyo, entered on a phone still set to Paris time, must land on the right day.** Five timestamps per expense, grouping by the local date of the place, `Date` banned in business code by a lint rule that has its own tests. A time that does not exist or happens twice (daylight saving) is never guessed. A phone whose clock is hours off is measured and flagged, never silently corrected.
- **"It works" is not evidence.** Virtual phones run the real engine against the real API while a fault proxy loses responses and breaks clocks, and each demo ends with a checked `PASS`. As generated on 2026-10-07: **274 tests**, a convergence property test over **150 random multi-device scenarios**, and a **mutation check that breaks the engine 6 ways and catches 6 of 6**. CI runs the suite under 5 machine time zones.

18 [ADRs](https://github.com/guillaume-lecomte/trip-ledger/tree/main/docs/decisions). Known limits: no real authentication, verified on an Android emulator and in a browser only, not on iOS or a physical device.

### Smaller illustrations

- **[wealth-api](https://github.com/guillaume-lecomte/wealth-api)**: banks, crypto platforms and insurers send events in different shapes, twice, late, or with another amount. One append-only journal, duplicates recognised, contradictions stored as adjustments next to the original.
- **[conference-room-booking-api](https://github.com/guillaume-lecomte/conference-room-booking-api)**: idempotency keys so a retried request does not book twice, cache-aside reads, events for side effects, documented behaviour under concurrency.
- **[car-selector-app](https://github.com/guillaume-lecomte/car-selector-app)**: Next.js, Hono, Drizzle, PostgreSQL; Zod at the API edge, integrity rules in the schema.

---

## How I lead

The same few rules apply whether the team is people, agents, or both.

- **Brief, then a definition of done a machine can check.** A lint rule, a property test, an eval gate in CI. If a rule matters, a tool enforces it.
- **A test that cannot fail proves nothing.** I break the system on purpose (mutation checks, `SIGKILL` on a worker, a degraded prompt) and require the checks to notice.
- **Irreversible actions need a human.** Agent tools that change something wait for approval; conflicting data goes to a person.
- **Decisions are written down.** ADRs let a newcomer, human or agent, disagree with specifics instead of guessing the reasons.
- **No number without a source.** Figures in my READMEs come from a script in the repository, and limits are listed next to results.

## Next

- **incident-copilot**: a second reference agent that triages production incidents from logs, metrics and traces, proposes a diagnosis and a remediation, and waits for human approval before acting. Planned, not started.
- **proto-to-mcp**: exposing existing gRPC microservices to AI agents through MCP, with typed tool schemas, auth, per-method permissions and audit logs. Not published here.

---

## Skills

**AI & agents**: agent architectures, tool calling, multi-agent orchestration, MCP integration with existing backends, evals, observability, guardrails, cost control.

**Engineering & architecture**: TypeScript / Node.js / NestJS, React / React Native, .NET Core / C#, Python. Microservices, API Gateway, gRPC / protobuf, event-driven design, offline-first and sync. MongoDB, PostgreSQL, Redis / BullMQ, message queues.

**DevOps & cloud**: Kubernetes, Docker, CI/CD, GitHub Actions. AWS, GCP, Azure.

**Leadership**: technical strategy tied to business goals, hiring, mentoring, team structure, work with founders, executives and investors, technical due diligence.

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=ts,nodejs,nestjs,react,py,cs,dotnet,mongodb,postgres,redis,docker,kubernetes,aws,gcp,azure,githubactions,linux,git" />
  </a>
</p>
