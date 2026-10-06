# Guillaume Lecomte

CTO / Lead Developer with **14+ years of experience** designing, building and scaling SaaS platforms and distributed systems.
I now focus on **agentic AI engineering**: the platforms, integrations and guardrails that let AI agents run reliably in production, on top of real-world architectures.

Based in France, working fully remotely. Open to **CTO, Fractional CTO or Head of AI Engineering** roles.
**[LinkedIn](https://www.linkedin.com/in/guillaumelecomtefr)** · **[guillaume-lecomte.fr](https://guillaume-lecomte.fr)**

---

## Where to start

If you only have two minutes, read the first project. The others show the same habits on smaller problems. Each README says what works, how I checked it, and what does not work yet.

### 1. [agent-platform](https://github.com/guillaume-lecomte/agent-platform): running LLM agents in production

A TypeScript monorepo (NestJS, BullMQ, PostgreSQL, Redis, React) with one small demo agent that triages GitHub issues. It runs locally on a built-in mock model, with no API key. It answers three problems I kept seeing:

- **An agent dies halfway through a task.** Every model call and tool result is saved as a step, so another worker resumes from the last step. Tools that change something wait for a human approval and run at most once. `pnpm crash-demo` kills a worker with `SIGKILL` and checks it in the database. See [durable-runner](https://github.com/guillaume-lecomte/agent-platform/tree/main/packages/durable-runner).
- **Every call goes to the biggest model.** A YAML routing policy picks the model by task type and complexity, with a fallback and an exact-match Redis cache, and every call is recorded with its cost and latency. See [cost-router](https://github.com/guillaume-lecomte/agent-platform/tree/main/packages/cost-router).
- **A prompt change quietly makes the agent worse.** A 40-case eval suite is scored against a committed baseline, CI fails when the score drops, and a promote and rollback command moves agent versions. A deliberately degraded prompt shows the gate blocking. See [evals](https://github.com/guillaume-lecomte/agent-platform/tree/main/packages/evals).

Design decisions are written down as [ADRs](https://github.com/guillaume-lecomte/agent-platform/tree/main/docs/decisions). Known limits: no authentication, a single Redis and PostgreSQL instance.

### 2. [wealth-api](https://github.com/guillaume-lecomte/wealth-api): one journal from many financial sources

A NestJS and MongoDB prototype. Banks, crypto platforms and insurers send events in different shapes, twice, late, or with a different amount. Each event is normalised into one append-only journal, duplicates are recognised, and a contradiction becomes an adjustment event that keeps the original, so the history that explains a balance stays intact. The README lists its limits.

### 3. [conference-room-booking-api](https://github.com/guillaume-lecomte/conference-room-booking-api): booking under retries and concurrency

An Express, PostgreSQL, Redis and RabbitMQ API for booking rooms: idempotency keys so a retried request does not book twice, a cache-aside layer for reads, and events for side effects. It is a demonstration project, and the README documents how it behaves under concurrent requests.

### 4. [car-selector-app](https://github.com/guillaume-lecomte/car-selector-app): a small full-stack TypeScript app

Next.js with a Hono API, Drizzle ORM and PostgreSQL: Zod validation at the API edge, integrity rules in the schema, paginated lists.

---

## From production experience to public code

The production work below is not published, so these projects are smaller illustrations of the same concerns, not copies of it.

| From my track record | The concern behind it | Where to see it in public code |
| --- | --- | --- |
| **-50% infrastructure costs** by replacing a legacy stack with modular microservices (NestJS + gRPC) | Clear service boundaries, typed contracts, cost you can see | agent-platform: a modular monorepo (core, durable-runner, cost-router, evals, API, dashboard), ADRs for the decisions, and cost and latency tracked per model call. gRPC itself is not in public code. |
| **10-15× faster data imports** (30 min → 2-3 min) through gRPC streaming | Moving large volumes without blocking users | Not in public code. |
| SaaS platform **adopted by EASA and 5 European airlines**, in a safety-critical, regulated sector | Traceability, no silent data loss, humans in the loop | wealth-api keeps every original event and records corrections next to it. agent-platform saves every step, and effectful tools wait for human approval. |
| Monorepo CI/CD with **independent deployments and granular rollback** | Shipping parts separately and undoing safely | agent-platform: Turborepo, an eval gate that only runs when the agent, the eval tooling or the suite changed, and per-agent promote and rollback. This is agent versions, not service deployments. |
| Built and structured engineering teams from first hire to CTO | Technical strategy, hiring, mentoring | Not in code. See LinkedIn. |

## Next

- **incident-copilot**: a second reference agent that triages production incidents from logs, metrics and traces, proposes a diagnosis and a remediation, and requires human approval before any action. It would reuse the runner, the router and the eval tooling of agent-platform. Planned, not started.
- **proto-to-mcp**: exposing existing gRPC microservices to AI agents through MCP, with typed tool schemas, auth, per-method permissions and audit logs. A separate project, not published here. The MCP client adapter in agent-platform is made to consume its tools.

---

## Skills & Expertise

### AI & Agents
- Agent architectures, tool calling and multi-agent orchestration
- MCP integration with existing backends and APIs
- Evals, observability, guardrails and cost control for LLM-based systems

### Engineering & Architecture
- Scalable, secure architectures for SaaS platforms
- Microservices, API Gateway, gRPC / protobuf, event-driven patterns
- **TypeScript / Node.js / NestJS**, React / React Native, .NET Core / C#, Python
- MongoDB, PostgreSQL, Redis / BullMQ, message queues

### DevOps & Cloud
- Kubernetes, Docker, CI/CD, GitHub Actions
- AWS, GCP, Azure · cloud-native tooling, observability

### Leadership & Product
- Technical strategy aligned with business goals
- Hiring, mentoring and team structuring
- Stakeholder management with founders, executives and investors, technical due diligence
- Agile delivery with a strong product mindset

---

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=ts,nodejs,nestjs,react,py,cs,dotnet,mongodb,postgres,redis,docker,kubernetes,aws,gcp,azure,githubactions,linux,git" />
  </a>
</p>
