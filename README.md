# 🛡️ Governance-First RAG Ingestion & Release Platform

> RAG operations infrastructure for teams running production RAG at scale — not a chatbot demo.

A governance-first ingestion admin that turns raw sources into auditable, production-ready RAG releases with connectors, health gates, quarantine, and observability.

---
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/47b9b633-0af2-4dfa-8a83-d2eda74229ea" />

## 🚩 Why This Exists

Most RAG systems fail after the demo — not because embeddings break, but because there is:

- ❌ No controlled release process
- ❌ No rollback when data regresses
- ❌ No audit trail or lineage
- ❌ No operational or compliance proof
  
<img width="1920" height="1080" alt="Screenshot 2026-01-07 190434" src="https://github.com/user-attachments/assets/4d8f84d5-76b7-4fd1-82b2-cb2daacea335" />



This platform addresses those gaps by providing a **control plane for ingestion, releases, and quality**.

---

## 🧠 What This Platform Is

An **end-to-end ingestion workflow**: capture → ingest → release → validate

A **release governance layer** with promotion, rollback, merge, and audit history

A **quality gate system** to catch regressions before production

A **compliance & observability surface** for audits and operations

**Designed to run on-prem or in private VPCs.**

---

## 🧩 Core Capabilities

### 1️⃣ Ship RAG Safely

- Guided ingestion runs (single, batch, file)
- Versioned releases per domain
- Promotion, rollback, merge, lock/unlock
- Retrieval smoke testing before activation

### 2️⃣ Keep Data Fresh with Connectors

- Connector-based ingestion (URL lists supported)
- Test → run → history per connector
- Domain-scoped connectors and releases

  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e1615b79-900a-460b-8451-cfd0659ba6ef" />


### 3️⃣ Catch Regressions Before Production

- Health gates and golden query execution
- Regression detection prior to promotion
- Quarantine center for blocking artifacts

### 4️⃣ Prove Compliance & Operations

- Server-side RBAC enforcement
- Permission-aware UI states
- Full audit trace lineage (source → doc → chunk)
- Observability runs with exportable audit packs


<img width="1920" height="1080" alt="Screenshot 2026-01-09 144430" src="https://github.com/user-attachments/assets/f3a26124-1bf8-4d01-ae45-e458a55e958c" />


---

## 🔐 Enterprise Configuration (Built In)

- **SSO (OIDC)** configuration and test
- **RBAC** role mapping and permission preview
- **Audit event** configuration and log destinations
- **Evidence pack** downloads (system + release)
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/153726db-95a4-4a07-b297-30f21a68ae66" />

---

## 📊 What's Actually Implemented

No speculative claims. No roadmap padding. From the code:

- ✅ 3 connector types: URL list, S3, local folder
- ✅ 8 connector API endpoints: CRUD, test, run, run history
- ✅ 15 configuration endpoints: SSO, RBAC, audit, evidence packs
- ✅ RBAC enforced server-side (not just UI)
- ✅ Full audit lineage for RAG units
- ✅ End-to-end workflow: capture → ingest → release → retrieve

---

## ⚠️ Known Limitations

- S3 and local folder connectors currently support test only
- Connector scheduling/automation not yet implemented
- System evidence pack is a configuration snapshot (logs roadmapped)
- Connector run history UI expects some fields not yet returned by API

This project favors **production honesty over demo theater**.

---

## 🏗️ Local Development

```bash
# Backend + frontend
./start_services.ps1

# Backend only
./start_backend.ps1
```

- **API**: http://127.0.0.1:8002
- **UI**: http://127.0.0.1:5173 or 5174

---

## 🎥 Demo Flow (10–12 minutes)

1. Select domain → observe scoped releases
   <img width="1920" height="1080" alt="Screenshot 2026-01-07 185105" src="https://github.com/user-attachments/assets/1ef11d84-7d61-4c85-b737-535a01053e46" />

3. Run ingestion → show counts + release ID
   <img width="1920" height="1080" alt="Screenshot 2026-01-07 185642" src="https://github.com/user-attachments/assets/aa52a2d1-68ab-4877-9b61-52a88b4ed04a" />

5. Promote release → governance checks enforced
6. Run golden queries → health gate results
   <img width="1920" height="1080" alt="Screenshot 2026-01-07 190415" src="https://github.com/user-attachments/assets/e50b1dbf-14e5-469d-86b9-f7bd099207c8" />

8. Open quarantine → blocking artifacts visible
   <img width="1920" height="1080" alt="Screenshot 2026-01-07 190926" src="https://github.com/user-attachments/assets/17aead79-ad2a-4f8d-9a6d-abba4bb43f77" />

10. Inspect audit trace → full lineage
11. Open observability → run timeline + artifacts
    <img width="1920" height="1080" alt="Screenshot 2026-01-07 190449" src="https://github.com/user-attachments/assets/8edc55f6-95ae-4dfc-904a-bc907cef76b6" />

13. Trigger RBAC denial → UI + API enforcement

**Everything shown is logged, auditable, and exportable.**

---

## 📌 What This Is (And Isn't)

| This is NOT | This IS |
|---|---|
| A chatbot builder | A RAG ingestion & release control plane |
| A vector database wrapper | An AI platform governance surface |
| A SaaS-only demo tool | Infrastructure for production RAG teams |

---

## 🧭 Roadmap

- [ ] Connector scheduling / automation
- [ ] Full S3 and local ingestion support
- [ ] Richer evidence packs (audit logs + metrics)
- [ ] Connector run history field alignment

---

## 📖 Documentation

For more details on setup, architecture, and usage, see the `/docs` directory.

---

## License

[Add your license here]

---

**Built for teams that take RAG operations seriously.**
