
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

I build systems that other systems depend on — high-throughput ingestion, deterministic matching engines, multi-tenant OLAP, real-time collaboration, and the boring infrastructure. Seven years across early/growth-stage and stealth companies, owning architecture end-to-end: from defining service boundaries and consistency models, down to JVM tuning and the SLOs that keep on-call quiet.

I care most about three things: **Building high throughput**, **low latency**, and **highly available** systems.

---

## Systems I've built (the ones worth talking about)

### Real-time ad analytics — 10k QPS sustained, click-to-query in ~70s

Kafka → Apache Flink → ClickHouse, with stateful stream processing, exactly-once checkpointing, and late-event handling. Brands query 1-minute granular campaign metrics within ~70 seconds of the click landing. Hot rollups cached in Redis with 30s TTL and click-level de-duplication, materially reducing ClickHouse query load.

**What was hard:** late events without breaking exactly-once. Flink's watermark + allowed lateness gives you the *mechanism*; the *policy* (how late is "too late" for an advertiser dashboard?) is a product decision dressed as an engineering one. I wrote the design doc that forced that conversation.

**SLOs I committed to and held:** 99% availability, click-to-query freshness < 90s, analytics read p99 < 200ms. Validated under k6 load tests and chaos drills — TaskManager kills mid-window, broker failovers — exactly-once recovery confirmed with zero data loss.

### Multi-tenant SaaS analytics — 1M+ events/sec, 10k+ tenants
> *Architecture & design work*

Go-based ingest gateway, schema registry, and query layer. Kafka partitioning tiered by tenant class. ClickHouse with tenant-aware sharding and S3-tiered storage (hot SSD → warm HDD → cold S3 via Apache Iceberg). OpenSearch for event-level lookups, Redis for query result caching.

**The interesting problem:** noisy-neighbor isolation without dedicated infrastructure per tenant. Solved via ClickHouse profile-level CPU/memory/concurrency quotas, with every query traced under a `tenant_id` span tag feeding per-tenant cost dashboards. You can't fix what you can't attribute.

### Stock trading platform — order management + matching engine
> *Personal architecture project*

Mini-Robinhood / Interactive Brokers, designed for 1M orders/sec with deterministic sub-microsecond matching. Hot path in C++/Rust (matching engine, market data handler at ~5M ticks/sec), cold path in Go/Java (clearing, compliance, audit).

**Why it's a challenging system I've designed:** it's the strictest intersection of *correctness, latency, and regulation* in software. A misplaced order costs millions and triggers regulatory inquiries. Closer to embedded systems than typical backend.

The architecture leans on:
- **LMAX Disruptor** for in-memory event processing on the hot path
- **Aeron / Chronicle Queue** for ultra-low-latency IPC between services
- **Single-writer per symbol** — no contention, cache-line friendly
- **Event sourcing with deterministic replay** — recreate any moment in market time, byte-for-byte
- **Pre-trade risk checks** (account margin, position limits, regulatory limits) on the gateway, before the order ever touches the matching engine
- **Kill switches at three layers**: per-account, per-symbol, global
- **PTP-synchronized microsecond timestamping** for SEC Rule 613 / CAT and MiFID II reporting
- **kdb+/QuestDB** for tick analytics, TimescaleDB for end-of-day, FIX protocol for exchange connectivity

### Real-time collaborative document platform
> *Architecture & design work*

Active-active multi-region, concurrent editors per document, keystroke-to-remote-echo p99 < 100ms within-region. Rust + Go microservices, Yjs / Automerge (CRDTs) for convergence, Redis Streams for ephemeral collab state, Postgres for metadata, S3 for content-addressed snapshots and version blobs.

WebSocket edge layer uses consistent hashing for sticky sessions to document shards. Convergence verified via Jepsen-style network partition testing.

### Streaming & AR video infrastructure

Low-latency AI avatar / lip-sync video pipeline in Go on GCP, HLS + WebSocket distribution for AR ad experiences. Green-screen service orchestrating concurrent FFmpeg workers on GKE, queue-driven scaling with back-pressure for high-volume rendering.

### Earlier work worth mentioning

- Built search & discovery read paths with Go worker pools — APIs scaled from struggling at lower volumes to 150k+ peak QPS, p99 from 450ms → <80ms. Built the CDC pipeline (Go + Kafka), keeping Elasticsearch within 200ms of the source of truth for the global feed.
- Subscription billing platform driving 50% revenue growth. Idempotent payment flows, webhook processing with retry, exactly-once semantics, and dead-letter recovery. The system I'm proudest of for being *boring* in production.
- CQRS analytics platform ingesting 20M records/day. Saga orchestration on RabbitMQ for distributed trip lifecycles with compensating transactions.

---

## How I work

**Design docs / System design before code.** Every system above started as a doc with explicit functional, non-functional requirements, failure modes, and SLO commitments.

**SLOs are contracts, not aspirations.** I believe if we can't define availability, latency, and freshness as numbers, we don't have a system — we just have a hope.

**Chaos before production.** Every critical path I've shipped has been tested with broker kills, TaskManager failures, network partitions, and replica loss *before* it ever served real traffic.

**Mentorship is system design.** The teams I've led — Backend, iOS/Swift, React Native — got architecture reviews, design doc templates, and clear escalation paths. Believing that, people are systems too; they have throughput, latency, and failure modes.

---

## Stack

Most used stacks till now - 

| Layer | Tools |
| :--- | :--- |
| **Application backend** — Java/Spring, Go, Node/TypeScript, Rust |
| **Streaming & messaging** | Kafka, Apache Flink, Redis Streams, RabbitMQ, Aeron |
| **OLAP & storage** | ClickHouse, Apache Iceberg, TimescaleDB, kdb+ / QuestDB, Postgres, Mongo/DynamoDB, S3 |
| **Search** | Elasticsearch, OpenSearch (edge n-grams, function score, decay) |
| **Infra & observability** | Kubernetes, GKE, AWS, GCP, Prometheus, Grafana, Zipkin |
| **Patterns** | Event sourcing, CQRS, Saga, DDD, CRDTs, multi-tenancy isolation |
| **Mobile** | Swift / SwiftUI, React Native, Kotlin(Android) | 

---

## What I'm open to

Founding or early-staff roles where the problems involve dealing with real-time, high-throughput systems involving AI. Financial infrastructure, developer platforms, or B2C at a scale where the architecture actually matters. Love reading papers and making system design docs.

📫 *You can reach out to me at - **shivakp2111@gmail.com** *
