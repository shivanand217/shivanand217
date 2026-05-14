
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
### Staff Software Engineer | Bengaluru, India

I am a Software Engineer with 6+ years of experience designing and building high-throughput, real-time data platforms and event-driven microservices at scale. Have built high-throughput ingestion pipelines, billing/payments, highly available, and low-latency video infrastructure.

My work focuses on solving complex synchronization, multi-tenancy isolation, and consistency problems in real-time environments.

---

## 🏗️ Most impactful projects.

### 1. Multi-Tenant SaaS Analytics Platform
*A high-scale OLAP engine designed for 1M+ events/second across 10k+ tenants with strict isolation.*

*   **Core Tech:** 
    *   **Services:** Go-based ingest-gateway, schema-registry, and query services.
    *   **Data:** Kafka (tier-based partitioning), ClickHouse (tenant-aware sharding + S3 tiered storage), and Apache Iceberg for cold storage.
    *   **Caching/Search:** Redis for query result caching and OpenSearch for event-level search.
    *   **Tiered Storage:** Hot SSD → Warm HDD → Cold S3 with intelligent query rewriting.
    *   **Isolation:** Enforced resource quotas (CPU, memory, query concurrency) via ClickHouse profiles.
    *   **Observability:** Every query is traced with a `tenant_id` span tag, feeding into per-tenant cost dashboards ($/MTU, $/query).

### 2. Real-Time Collaborative Document Platform
*An active-active, multi-region collaborative engine supporting concurrent editors per document with <100ms latency.*

*   **Core Tech:** 
    *   **Services:** Rust and Go microservices for gateway, document, and presence management.
    *   **Consistency:** Yjs/Automerge (CRDTs) and Redis Streams for ephemeral collab state.
    *   **Persistence:** PostgreSQL for metadata; S3 for content-addressed doc snapshots and version blobs.
    *   **Convergence & Chaos:** Utilized Jepsen-style testing to prove replica convergence under extreme network partitions.
    *   **Connection Management:** Engineered a WebSocket edge layer using consistent hashing to maintain sticky sessions to document shards.
    *   **Performance:** Achieved keystroke-to-remote-echo p99 < 100ms within-region through snapshot + delta persistence strategies.

---

## 🛠️ Technical Ecosystem

| Domain | Technologies |
| :--- | :--- |
| **Backend Core** | Go, Rust, Java, Distributed Systems Design |
| **Data & Messaging** | Kafka, Apache Flink, Redis Streams, RabbitMQ |
| **Storage & OLAP** | ClickHouse, PostgreSQL, Apache Iceberg, OpenSearch, S3 |
| **Search & Optimization** | Elasticsearch (Edge N-gram tokenizers, Function Score queries) |
| **Architecture** | CQRS, Saga, Event-Driven Microservices, CRDTs, Multi-tenancy |
| **Web/Mobile** | iOS(Swift), React-native, NextJs, React.

---
<p align="left">
  <img src="https://github-readme-stats.vercel.app/api?username=shivanand217&show_icons=true&theme=tokyonight" alt="GitHub Stats" />
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=shivanand217&theme=tokyonight" alt="Contribution Streak" />
</p>
---
*Based in Bengaluru, India. I am particularly interested in entrepreneurial ventures and exploring massive B2C product ideas that solve underserved market gaps.*
