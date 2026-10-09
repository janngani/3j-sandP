# Diagram 10 – UML Deployment (Provisional): PrintAhead hosting

**Scope:** where each artifact will run. **Provisional:** no hosting provider has been chosen, so node names are generic.
**Primary audience:** the team and the instructor (for the Deployment unit).
**Risk it reduces:** discovering late that something cannot be hosted, or that a connection (such as uploads to storage) was never planned.

```mermaid
---
title: "Deployment diagram (Provisional): PrintAhead hosting"
---
flowchart TB
  subgraph UD["Node: User device (laptop or phone)"]
    BR["Execution environment: Web browser<br/>Artifact: Next.js client bundle (UI)"]
  end

  subgraph HOST["Node: Application hosting platform (provider not chosen)"]
    RT["Execution environment: Node.js runtime<br/>Artifact: Next.js server bundle (API route handlers)"]
  end

  subgraph DBN["Node: Managed database server (provider not chosen)"]
    PG["Execution environment: PostgreSQL<br/>Artifact: schema and migrations"]
  end

  subgraph OBJ["Node: Object storage service (provider not chosen)"]
    BKT["Execution environment: S3-compatible storage<br/>Artifact: bucket of uploaded files"]
  end

  IDP["Node: SorSU email identity provider<br/>(external)"]
  EML["Node: Email delivery service<br/>(external)"]

  style UD fill:#ffffff,stroke:#444444
  style HOST fill:#ffffff,stroke:#444444
  style DBN fill:#ffffff,stroke:#444444
  style OBJ fill:#ffffff,stroke:#444444

  HOST -->|"HTTPS: serves pages and client bundle"| UD
  BR -->|"HTTPS/JSON: API calls"| RT
  BR -->|"HTTPS (OIDC redirect)"| IDP
  RT -->|"HTTPS (OIDC)"| IDP
  RT -->|"PostgreSQL protocol over TLS"| PG
  RT -->|"HTTPS (S3 API)"| BKT
  RT -->|"HTTPS (email API)"| EML
```

## Key
| Notation | Meaning |
|---|---|
| Large frame | Node (a device or server) |
| Box inside a node | Execution environment and the artifact deployed on it |
| Arrow | Communication path, labelled with its protocol |

## Notes
- No secrets, keys, or real addresses are shown. Store them as environment variables on the hosting platform.
