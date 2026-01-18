# Ingestion Service

> **Governed RAG ingestion + retrieval** — A production-ready control plane for building auditable knowledge bases with release workflows, quarantine handling, and comprehensive governance.

Knowledge is not a script. It's a **production system**.

---

## 🎯 What is this?

A modular ingestion control plane that transforms raw content into governed, retrievable knowledge. Ingest from URLs, uploads, and connectors. Apply policies, chunk intelligently, generate embeddings, and ship through release gates—all with full audit trails and RBAC.

Unlike typical RAG pipelines that hard-wire a chunker, embedding model, and vector database into a single script, this system treats ingestion as **software delivery**:

- Content moves through a **release lifecycle** (ingest → validate → promote → rollback)
- Every stage is **pluggable and policy-driven**
- Quality, safety, and compliance are enforced through **gates**
- All actions are **observable and auditable**

**Perfect for:** Platform engineers, security teams, and AI teams building RAG infrastructure at scale.

---

## ✨ Key Features

| Feature | What it does |
|---------|--------------|
| **Release Lifecycle** | Create → Validate → Promote → Rollback. Manage knowledge like software. |
| **Quarantine Workflows** | Automatically flag suspicious or policy-breaking inputs. Apply manual fixes. |
| **Health Gates + Golden Queries** | Validate ingestion and retrieval quality before promoting to production. |
| **Domain-Scoped Governance** | Isolated namespaces with their own policies, connectors, and RBAC. |
| **Audit & Evidence Packs** | Download compliance reports, lineage traces, and decision records. |
| **Pluggable Backends** | Swap embeddings, vector stores, and chunking strategies without rewrites. |
| **Observability** | Step-level telemetry, run history, and append-only event logs. |
| **Enterprise Auth** | OIDC SSO + domain-scoped RBAC. |

---

## 🏗 Core Concepts

### Domains
A **Domain** is a governed namespace for knowledge—e.g., `finance`, `security`, `support`, `engineering`.

Each domain has:
- Its own connectors and ingestion sources
- Its own chunking and embedding policies
- Its own RBAC scope and audit history
- Its own releases and vector stores

**Why?** Isolation, compliance, and ownership boundaries.

### Releases
A **Release** is a versioned snapshot of your knowledge base.

Every ingestion run produces a release that moves through stages:
```
capture → distill → canonicalize → chunk → embed → index → validate → promote
```

Releases can be promoted, merged, locked, rolled back, and audited—just like code.

### Artifacts
Immutable outputs produced by each ingestion stage:

| Stage | Output | Purpose |
|-------|--------|---------|
| **Capture** | Raw HTML / files | Source evidence |
| **Distill** | Clean text | Noise removal |
| **Canonicalize** | Structured objects | Normalized content |
| **Chunk** | Chunk JSON | Retrieval units |
| **Embed** | Vector arrays | Semantic representation |
| **Index** | Vector index | Queryable store |

All artifacts are linked to releases, domains, and audit traces.

### Gates
Quality and safety checkpoints that must pass before promotion:

- **Quarantine Gate** — Isolate suspicious inputs
- **Health Gate** — Validate ingestion quality (chunk counts, embedding success rates)
- **Golden Queries** — Validate retrieval quality before promotion
- **PII Gate** — Block sensitive data and require override justification

---

## 🚀 Quick Start

### Docker (Recommended)

```bash
cd deploy
docker compose up -d --build
```

Open http://localhost:3000 in your browser.

### Local Development

**Backend:**
```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .\.venv\Scripts\activate
pip install -r requirements.txt
python -m uvicorn api:app --host 127.0.0.1 --port 8000 --reload
```

**Frontend:**
```bash
cd frontend
npm install
npm run dev
```

---

## 🔄 Core Workflows

### Ingest & Promote
1. **Capture** raw content (URLs, files, connectors)
2. **Ingest** into a release (normalize, chunk, embed)
3. **Quarantine** suspicious items (policy violations, low quality)
4. **Review** and apply fixes
5. **Gate** with health checks and golden queries
6. **Promote** to active release for retrieval

### Query Your Knowledge Base
```bash
POST /retrieve
{
  "query": "What is your refund policy?",
  "domain": "support",
  "release_id": "prod-v1.2"  # or omit for active release
}
```

Every query is audited and can be explained:
```
query → vectors → chunks → documents → sources → release
```

---

## 🏭 Modular Ingestion Pipeline

The pipeline is explicitly modular and policy-driven:

```
[Connector] → Distiller → Canonicalizer → Chunker → Embeddings → Vector Store
```

Each layer is swappable without rewriting the system.

### Distillation Layer (pluggable)
Convert raw sources into clean text.
- HTML cleaning, PDF extraction, Markdown normalization
- Custom enterprise document filters

### Canonicalization Layer
Normalize content into structured, deterministic objects.
- Section detection, metadata enrichment, language normalization
- Source attribution for audit trails

### Chunking Layer (policy-driven)
Break content into retrieval-safe units with domain-specific policies.
```json
{
  "policy": "default",
  "max_chars": 2500,
  "overlap": 200,
  "semantic": true
}
```

### Embeddings Layer (pluggable backends)
Convert chunks into vectors.
- **Local models** — Run embeddings in-process
- **Cloud APIs** — OpenAI, Azure, Bedrock, Vertex
- **Custom services** — Bring your own embedding backend

### Vector Store Layer (pluggable backends)
Index and retrieve vectors at scale.
- **Local** — FAISS
- **Cloud-native** — Qdrant, Weaviate, Pinecone
- **Enterprise** — Custom search services

---

## 📊 Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      User Browser                            │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐    │
│  │  React + TypeScript + Vite                           │    │
│  │  (UI for ingestion, releases, quarantine, queries)   │    │
│  └──────────────────┬───────────────────────────────────┘    │
└─────────────────────┼──────────────────────────────────────────┘
                      │ HTTP/REST
        ┌─────────────▼──────────────────┐
        │  Nginx Reverse Proxy (SSL/TLS) │
        └─────────────┬──────────────────┘
                      │
        ┌─────────────▼──────────────────────────────────────┐
        │   FastAPI Backend                                   │
        │  ┌───────────────────────────────────────────────┐  │
        │  │ Ingestion Pipeline (modular stages)           │  │
        │  │ Release Management + Promotion                │  │
        │  │ Quarantine Workflows                          │  │
        │  │ Retrieval Service + Query Audit               │  │
        │  │ OIDC + Domain-Scoped RBAC                     │  │
        │  │ Observability + Event Streaming               │  │
        │  └───────────────────────────────────────────────┘  │
        └─────────────┬──────────────────────────────────────┘
                      │
        ┌─────────────┴──────────────────────────┐
        │                                         │
   ┌────▼────────┐      ┌──────────────────┐   │
   │  SQLite DBs │      │ File Storage     │   │
   │             │      │ (artifacts,      │   │
   │ • audit     │      │  chunks,         │   │
   │ • traces    │      │  embeddings,     │   │
   │ • telemetry │      │  vector index)   │   │
   └─────────────┘      └──────────────────┘   │
        │
        └──────────────────────────────────────┘
```

---

## 📁 Project Structure

```
.
├── api.py                      # FastAPI application (all routes)
├── pipeline.py                 # Ingestion orchestration
├── releases.py                 # Release lifecycle management
├── quarantine.py               # Quarantine store & actions
├── retrieval_service.py        # Query & retrieval logic
├── observability.py            # JSONL event logging
├── oidc.py                     # Authentication & sessions
├── config.py                   # SSO/RBAC/audit configuration
├── env.py                      # Runtime paths
├── frontend/                   # React + TypeScript + Vite + Tailwind
├── deploy/                     # Docker Compose + Nginx config
├── db/                         # SQLite migrations
├── docs/                       # Schemas & design notes
├── data/                       # Default runtime data root
└── tests/                      # Test suite
```

---

## 💾 Storage & Persistence

Data lives under `INGESTION_DATA_ROOT` (default: `./data`):

```
data/
├── captures/           # Raw ingestion artifacts
├── canonical/          # Normalized intermediate objects
├── chunks/             # Processed chunks per release
├── embeddings/         # Embedding vectors
├── vector_index/       # Vector store/index files
├── releases/           # Release metadata & active pointers
├── quarantines/        # Quarantine records & decisions
├── observability/      # JSONL event logs + metadata DB
└── config/             # SSO, RBAC, audit, log destinations
```

**Databases:**
- `observability/metadata.sqlite` — Internal telemetry
- `audit_traces/trace_events.sqlite3` — Full audit trail

---

## 🔐 Security & Access Control

### OIDC SSO
Configure via the UI setup flow. Works with any OIDC provider:
- Keycloak, Okta, Auth0, Azure AD, Google

### Domain-Scoped RBAC
- Assign roles per domain
- Backend enforces permissions for sensitive operations
- Custom claim mappings supported

### Configuration
```
INGESTION_DATA_ROOT/config/
├── oidc.json         # OIDC provider settings + claim mappings
├── rbac.json         # Role-to-permission mappings per domain
├── audit.json        # Event families & retention policies
└── log_destinations/ # Where to ship events (Splunk, DataDog, etc.)
```

### Secrets Management
- Use secret references (env/file) in production
- No plaintext secrets in JSON config

### Trust Boundaries
```
Browser → Nginx (TLS) → FastAPI → Storage / SQLite
```

**Production hardening:**
- Run behind TLS + trusted reverse proxy
- Lock down dev-mode auth endpoints
- Prevent header spoofing with perimeter controls
- Configure log shipping to external systems

---

## 📡 Observability & Auditability

### Observability Model
- Append-only JSONL events per domain
- Run-level and step-level telemetry
- Normalized event schema for analysis

### Audit Model
- SQLite trace store with full lineage
- Artifact provenance tracking
- Evidence packs for compliance and review

### Query Audit
Every retrieval query is traced:
- Which release was used
- Which chunks were returned
- Which documents were sourced
- User identity and timestamp
- Quality signals and warnings

Download evidence packs for compliance audits.

---

## 🛠 Configuration

### Environment Variables

```bash
# Data storage
INGESTION_DATA_ROOT=./data

# OIDC (optional, configure via UI if not set)
OIDC_ISSUER=https://your-idp.com
OIDC_CLIENT_ID=your-client-id
OIDC_CLIENT_SECRET=your-secret

# Logging
LOG_LEVEL=INFO
```

### Runtime Config Files

All config persists in `INGESTION_DATA_ROOT/config/`:

- **`oidc.json`** — OIDC provider & claim mappings
- **`rbac.json`** — Group-to-role, default roles
- **`audit.json`** — Enabled event families, retention
- **`log_destinations/`** — External log shipping config

---

## 📖 API Overview

### Ingestion

```bash
POST /ingest
{
  "domain": "support",
  "source": "url" | "upload" | "connector",
  "input": {...},
  "release_id": "my-release"
}
```

### Quarantine Management

```bash
GET /domains/{domain}/quarantine/{release_id}
POST /domains/{domain}/quarantine/{release_id}/{item_id}/action
{
  "action": "retry" | "ignore" | "bypass",
  "reason": "..."
}
```

### Health Gates

```bash
POST /releases/{release_id}/validate
{
  "golden_queries": ["query1", "query2"],
  "quality_threshold": 0.8
}
```

### Promote to Production

```bash
POST /domains/{domain}/releases/{release_id}/promote
```

### Query Knowledge Base

```bash
POST /retrieve
{
  "query": "...",
  "domain": "support",
  "release_id": "prod-v1.2",  # optional, uses active if omitted
  "top_k": 5
}
```

### Explain Query Results

```bash
GET /retrieve/{query_id}/explain
# Returns: query → vectors → chunks → documents → sources → release
```

---

## 🔌 Extension Guide

### Add a New Embedding Backend

1. Implement `EmbeddingBackend` interface
2. Register backend in config
3. Configure domain to use it

### Add a New Vector Store Backend

1. Implement `VectorStore` interface
2. Register backend in config
3. Configure domain to use it

### Add a New Gate

1. Implement gate interface
2. Emit observability + audit events
3. Register gate in promotion flow

### Add a New Connector

1. Implement connector interface
2. Add validation + test support
3. Register in UI

---

## 📦 Deployment Models

### Local Development
```bash
python -m uvicorn api:app --reload
cd frontend && npm run dev
```

### Docker Compose
```bash
cd deploy
docker compose up -d --build
```

### Kubernetes (Production)
- Backend service (StatefulSet)
- Frontend service (Deployment)
- Persistent volume for artifacts
- OIDC IdP integration (external)
- Log aggregation sidecar

---

## 🧪 Testing

```bash
pytest tests/
pytest tests/ -v --cov
```

---

## 🤝 Contributing

This repo is built around **release-based ingestion governance**. When modifying ingestion logic:

- ✅ Preserve release boundaries
- ✅ Emit audit traces
- ✅ Normalize observability events
- ✅ Use deterministic artifact paths

---

## 📄 License

(Add your license here.)

---

## 📞 Support

- **Issues:** GitHub Issues
- **Docs:** `/docs` folder for schemas and design notes
- **Architecture:** See [Feature Architecture Guide](./docs/architecture.md)

---

**Built for teams that treat knowledge as regulated production systems.**

> Ingestion is not a script. It is a control plane.
