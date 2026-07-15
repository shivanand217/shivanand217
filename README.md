
<!--
**shivanand217/shivanand217** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

# Hi there, I'm Shiv Prakash 👋
### Senior Software Engineer | Bengaluru, India

I build systems that other systems depend on — high-throughput ingestion, deterministic matching engines, multi-tenant OLAP, real-time collaboration. Seven years across early/growth-stage and stealth companies, owning architecture end-to-end: from defining service boundaries and consistency models, down to JVM tuning and the SLOs that keep on-call quiet.

I care most about three things: **Building high throughput**, **low latency**, and **highly available** systems.

---

## Systems I've built (the ones worth talking about)

### Real-time ad events lakehouse & analytics - 10k-15k sustained QPS, click-to-query in ~60s

Kafka → Apache Flink → ClickHouse, with stateful stream processing, exactly-once checkpointing, and late-event handling. Brands query 1-minute granular campaign metrics within ~70 seconds of the click landing. Hot rollups cached in Redis with 30s TTL and click-level de-duplication, materially reducing ClickHouse query load.

**What was hard:** late events without breaking exactly-once. Flink's watermark + allowed lateness gives you the *mechanism*; the *policy* (how late is "too late" for an advertiser dashboard?) is a product decision dressed as an engineering one. I wrote the design doc that forced that conversation.

**SLOs I committed to and held:** 99% availability, click-to-query freshness < 90s, analytics read p99 < 200ms. Validated under k6 load tests and chaos drills — TaskManager kills mid-window, broker failovers — exactly-once recovery confirmed with zero data loss.

### Sales-outreach agent

Multi-tenant AI-agent system: A B2B lead fills out a "Request a demo" form and an autonomous agent enriches them, scores ICP fit, researches, writes a personalized email, handles the multi-day reply conversation, and books a meeting — remembering everything about the lead across days and threads. The hard part isn't calling the LLM — it's making a non-deterministic agent safe to point at real customers' inboxes and calendars.

**Stateless loop, externalized memory.** The agent holds *no* state between runs — every invocation is one pass of a 6-step heartbeat (load short-term → load structured facts → retrieve semantic memory → assemble context → think→act→observe → write back), with all state in Redis (working buffer), Postgres (lead facts), and pgvector (semantic memory = RAG over the agent's own past). A reply that lands two days later wakes a fresh worker that behaves like one attentive salesperson — because memory is the agent.

**Cost discipline** A model-routing gateway sends cheap work (ICP scoring, reply-intent classification) to Claude Haiku and reserves Opus for nuanced outreach — ~2.5–3× cheaper per lead than an all-frontier baseline — with adaptive thinking, prompt-prefix caching, an exact-response cache, and per-(tenant,model) token budgets. Every call logs tokens + USD + latency, and runaway agents are capped by max-iterations and a per-run cost ceiling.


---

### Discovery & media infrastructure at Trell — 150k+ peak QPS, p99 450ms → <80ms
 
The read path for a TikTok-style content-commerce app's global feed. Two tightly coupled problems: serving discovery at scale and keeping a search index fresh against a firehose of engagement signals.
 
**The discovery engine.** Built in Go, backed by Elasticsearch with index aliases for zero-downtime re-indexing. Edge n-gram tokenizers for prefix/typo-tolerant matching, and Function Score queries with Gaussian decay to balance two things that pull in opposite directions — *freshness* (new content should surface) and *virality* (engagement should rank). Tuning that decay curve is the whole game: too aggressive and stale-but-popular content dominates; too soft and the feed feels random.
 
**What was hard:** keeping the index within 200ms of the source of truth at that signal volume. I built a Change Data Capture pipeline in Go + Kafka to asynchronously sync high-velocity video engagement signals and metadata into Elasticsearch, holding <200ms replication lag for the global feed. CDC over dual-writes because the feed can tolerate slight staleness but *cannot* tolerate the index and the source disagreeing — CDC gives you one ordered log of truth to replay from.
 
**Scaling the read path.** Re-engineered the discovery APIs around Go worker pools and goroutines, scaling from struggling at lower volumes to **150k+ peak QPS** and dropping **p99 from 450ms to <80ms**. The win wasn't a single trick — it was bounded concurrency (worker pools instead of unbounded goroutine spawn), connection reuse, and cutting redundant Elasticsearch round-trips on the hot path.
 
**The media pipeline.** An asynchronous video transcoding pipeline orchestrating multi-bitrate processing for 2k+ daily uploads. S3 multipart uploads + FFmpeg generating HLS adaptive streams (360p → 1080p) for seamless playback across network conditions.
 
**iOS.** Built the feed API contracts and rebuilt the iOS video feed (MVVM + Combine), contributing to 3× MAU growth (~400k users), and migrated the entire app from React Native to native iOS (Swift, SwiftUI).

### Earlier work worth mentioning

- Built search & discovery read paths with Go worker pools — APIs scaled from struggling at lower volumes to 150k+ peak QPS, p99 from 450ms → <80ms. Built the CDC pipeline (Go + Kafka), keeping Elasticsearch within 200ms of the source of truth for the global feed.
- Subscription billing platform driving 50% revenue growth. Idempotent payment flows, webhook processing with retry, exactly-once semantics, and dead-letter recovery.
- CQRS analytics platform ingesting 20M records/day. Saga orchestration on RabbitMQ for distributed trip lifecycles with compensating transactions.
- Built mobile apps/SDKs for insurtech, content-commerce(similar to tiktok) and healthcare products.

---

## How I work

**Design docs / System design before code.** Every system above started as a doc with explicit functional, non-functional requirements, failure modes, and SLO commitments.

**SLOs are contracts, not aspirations.** I believe if we can't define availability, latency, and freshness as numbers, we don't have a system — we just have a hope.

**Chaos before production.** Every critical path I've shipped has been tested with broker kills, TaskManager failures, network partitions, and replica loss *before* it ever served real traffic.

**Provide Mentorship in system design.** The teams I've led — Backend, iOS/Swift, React Native — got architecture reviews, design doc templates, and clear escalation paths. Believing that, people are systems too; they have throughput, latency, and failure modes.

---

## Stack

Most used stacks till now - 

| Layer | Tools |
| :--- | :--- |
| **Application backend** | Java/Spring, Go, Node/TypeScript, Rust, C++ |
| **Streaming & messaging** | Kafka, Apache Flink, Redis Streams, RabbitMQ, Aeron |
| **OLAP & storage** | ClickHouse, Apache Iceberg, TimescaleDB, kdb+ / QuestDB, Postgres, Mongo/DynamoDB, S3 |
| **Search** | Elasticsearch, OpenSearch (edge n-grams, function score, decay) |
| **Infra, Cloud and observability** | Kubernetes, GKE, AWS, GCP, Prometheus, Grafana, Zipkin |
| **LLM & AI infra** | vLLM, Temporal, OpenAI / Anthropic APIs, eval harnesses |
| **Patterns** | Event sourcing, CQRS, Saga, DDD, CRDTs, multi-tenancy isolation |
| **Mobile** | iOS(Swift), React Native, Kotlin(Android) | 

---

## What I'm open to

Senior, early-staff, or founding engineer roles, where the problems involve dealing with real-time, high-throughput systems involving AI. Financial infrastructure, developer platforms, or B2C at a scale where the architecture actually matters.

📫 *Can be reached out at - **shivakp2111@gmail.com** *
