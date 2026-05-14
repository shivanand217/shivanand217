
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
### Staff Distributed Systems Engineer | Bengaluru, India

I am a Software Engineer with over 6 years of experience designing and building high-throughput, distributed backend systems. I specialize in architecting event-driven microservices and resilient data pipelines using patterns like **CQRS** and **Saga** to ensure system reliability at massive scale.

My work focuses on solving complex synchronization, multi-tenancy isolation, and consistency problems in real-time environments.

---

## 🏗️ Architectural Deep Dives

### 1. Multi-Tenant SaaS Analytics Platform (Mini-Mixpanel/Amplitude)
*A high-scale OLAP engine designed for 1M+ events/second across 10k+ tenants with strict isolation.*

*   **The Challenge:** Ingesting 1M+ events/second while serving sub-second OLAP queries. The core difficulty lies in multi-tenancy—preventing "noisy neighbors" and managing cost-efficient tenant isolation (free tier vs. enterprise).
*   **Core Tech:** 
    *   **Services:** Go-based ingest-gateway, schema-registry, and query services.
    *   **Data:** Kafka (tier-based partitioning), ClickHouse (tenant-aware sharding + S3 tiered storage), and Apache Iceberg for cold storage.
    *   **Caching/Search:** Redis for query result caching and OpenSearch for event-level search.
*   **Staff-Level Rigor:**
    *   **Tiered Storage:** Hot SSD → Warm HDD → Cold S3 with intelligent query rewriting.
    *   **Isolation:** Enforced resource quotas (CPU, memory, query concurrency) via ClickHouse profiles.
    *   **Observability:** Every query is traced with a `tenant_id` span tag, feeding into per-tenant cost dashboards ($/MTU, $/query).

### 2. Real-Time Collaborative Document Platform (Mini-Google Docs)
*An active-active, multi-region collaborative engine supporting 100+ concurrent editors per document with <100ms latency.*

*   **The Challenge:** Scaling to 10M concurrent users globally while maintaining document convergence. This requires advanced distributed systems patterns to handle WebSocket scaling and multi-region conflict resolution.
*   **Core Tech:** 
    *   **Services:** Rust and Go microservices for gateway, document, and presence management.
    *   **Consistency:** Yjs/Automerge (CRDTs) and Redis Streams for ephemeral collab state.
    *   **Persistence:** PostgreSQL for metadata; S3 for content-addressed doc snapshots and version blobs.
*   **Staff-Level Rigor:**
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
| **Web/Mobile** | Swift, SwiftUI, React (Performance-critical backend integration) |

---

## 📊 Engineering Impact
Inspired by the visual metrics in **Screenshot 2026-05-15 at 12.15.39 AM.png**, these stats reflect a commitment to continuous delivery and architectural excellence.

<p align="left">
  <img src="https://github-readme-stats.vercel.app/api?username=[YOUR_USERNAME]&show_icons=true&theme=tokyonight" alt="GitHub Stats" />
  <img src="https://github-readme-streak-stats.herokuapp.com/?user=[YOUR_USERNAME]&theme=tokyonight" alt="Contribution Streak" />
</p>

---
*Based in Bengaluru, India. I am particularly interested in entrepreneurial ventures and exploring massive B2C product ideas that solve underserved market gaps.*
