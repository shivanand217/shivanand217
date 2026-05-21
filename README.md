
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

### Real-time ad analytics — 10k QPS sustained, click-to-query in ~70s

Kafka → Apache Flink → ClickHouse, with stateful stream processing, exactly-once checkpointing, and late-event handling. Brands query 1-minute granular campaign metrics within ~70 seconds of the click landing. Hot rollups cached in Redis with 30s TTL and click-level de-duplication, materially reducing ClickHouse query load.

**What was hard:** late events without breaking exactly-once. Flink's watermark + allowed lateness gives you the *mechanism*; the *policy* (how late is "too late" for an advertiser dashboard?) is a product decision dressed as an engineering one. I wrote the design doc that forced that conversation.

**SLOs I committed to and held:** 99% availability, click-to-query freshness < 90s, analytics read p99 < 200ms. Validated under k6 load tests and chaos drills — TaskManager kills mid-window, broker failovers — exactly-once recovery confirmed with zero data loss.

### Streaming & AR video infrastructure

Low-latency AI avatar / lip-sync video pipeline in Go on GCP, HLS + WebSocket distribution for AR ad experiences. Green-screen service orchestrating concurrent FFmpeg workers on GKE, queue-driven scaling with back-pressure for high-volume rendering.

### Multilingual AI Avatar Platform — RAG + lip-sync video
> *Shipped — banking & financial services use cases*
 
A platform that turns a person's likeness into a multilingual conversational avatar: the avatar speaks any supported local language with accurate lip-sync, grounded in domain knowledge via RAG. Built for banking and financial-services use cases — onboarding, customer support, branch-style assistance — where trust, language coverage, and factual accuracy are non-negotiable.
 
**Generation pipeline.** Go services on GCP / GKE orchestrating the full chain: text → TTS in target language → lip-sync video synthesis → HLS / WebSocket delivery. Concurrent FFmpeg worker pools for green-screen compositing and final encode. Queue-driven autoscaling with back-pressure so a burst of avatar generation requests doesn't melt the GPU pool — the same pattern I use for any heavy async workload, applied to GPU-bound work.
 
**RAG for grounded responses.** Hybrid retrieval over domain corpora — product docs, policy PDFs, branch FAQs — with dense embeddings + BM25 + reranking. Crucial detail: **retrieval and generation run in the user's chosen language**, not English-then-translate. Cross-lingual embeddings let a Hindi or Tamil query retrieve from an English source doc when needed, with the generation model producing the response in the target language. Chunking strategy tuned per document type (regulatory text vs. conversational FAQs vs. tabular product matrices).
 
**Multilingual is a systems problem, not a model problem.** TTS voice cloning per persona, accent and prosody calibration per language, lip-sync model conditioned on phoneme sequences that differ across languages — each one is a pipeline stage with its own latency, cost, and failure mode. The orchestrator treats every stage as a versioned, retryable activity so a TTS failure in Marathi doesn't break the entire request.
 
**Streaming delivery for AR.** HLS adaptive bitrate for pre-rendered avatar segments, WebSocket distribution for interactive/streamed responses, designed for the AR ad and in-app experiences Flam ships at scale. The low-latency path matters — a 2-second pause between question and avatar reply breaks the illusion of conversation.
 
**The hard problems.**
- *Lip-sync fidelity across phoneme inventories.* Hindi has retroflex consonants English doesn't; a model trained primarily on English produces uncanny output. Conditioning and post-processing matter.
- *Grounding without hallucination in regulated domains.* A banking avatar that invents an interest rate is a compliance incident. Retrieval-augmented generation with citation traces and refuse-to-answer fallbacks.
- *Cost per minute of avatar video.* Tracked at the workload level — TTS cost + LLM tokens + GPU-seconds for synthesis + bandwidth — so each banking customer use case has a defensible unit economics story.
- *Cold-start latency.* GPU warm pools sized against expected traffic; speculative pre-rendering of likely follow-up responses for common conversation flows.

### Earlier work worth mentioning

- Built search & discovery read paths with Go worker pools — APIs scaled from struggling at lower volumes to 150k+ peak QPS, p99 from 450ms → <80ms. Built the CDC pipeline (Go + Kafka), keeping Elasticsearch within 200ms of the source of truth for the global feed.
- Subscription billing platform driving 50% revenue growth. Idempotent payment flows, webhook processing with retry, exactly-once semantics, and dead-letter recovery. The system I'm proudest of for being *boring* in production.
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
