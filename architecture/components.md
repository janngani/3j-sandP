# Diagram 9 – UML Component: API Application internals

**Scope:** the components inside the API container (Diagram 2), their provided and required interfaces, and the external services behind adapters.
**Primary audience:** back-end developers.
**Risk it reduces:** hard-wired external services that cannot be replaced or tested, and business rules scattered across controllers.

```mermaid
---
title: "UML Component diagram: API Application"
---
flowchart TB
  subgraph API["API Application (Next.js route handlers, Node.js)"]
    direction TB
    RC["Request Controller"]
    DC["Delivery Controller"]
    AG["Auth Guard"]
    iSV(("SessionVerifier"))
    iRS(("RequestService"))
    iDS(("DeliveryService"))
    RS["Request Service"]
    DS["Delivery Service"]
    iPS(("PaymentService"))
    iSR(("StatusRules"))
    PS["Pricing and Payment Service"]
    SR["Status Rules"]
    iRP(("RecordStore"))
    iFS(("FileStoragePort"))
    iNP(("NotificationPort"))
    iID(("IdentityPort"))
    RP["Repositories"]
    SA["Storage Adapter"]
    NA["Notification Adapter"]
    IA["Identity Adapter"]
  end

  DB[("Database<br/>PostgreSQL")]
  FS[("File Storage<br/>S3-compatible")]
  EML["Email Notification Service<br/>(external)"]
  IDP["SorSU Email Identity Provider<br/>(external)"]

  %% required interfaces (dashed arrow: consumer needs the interface)
  RC -.-> iSV
  RC -.-> iRS
  DC -.-> iSV
  DC -.-> iDS
  AG -.-> iID
  RS -.-> iPS
  RS -.-> iSR
  RS -.-> iRP
  RS -.-> iFS
  RS -.-> iNP
  DS -.-> iPS
  DS -.-> iSR
  DS -.-> iRP
  DS -.-> iNP
  PS -.-> iRP

  %% provided interfaces (solid line: provider offers the interface)
  iSV --- AG
  iRS --- RS
  iDS --- DS
  iPS --- PS
  iSR --- SR
  iRP --- RP
  iFS --- SA
  iNP --- NA
  iID --- IA

  RP -->|"SQL over TLS"| DB
  SA -->|"HTTPS (S3 API)"| FS
  NA -->|"HTTPS (email API)"| EML
  IA -->|"HTTPS (OIDC)"| IDP

  style API fill:#ffffff,stroke:#444444
  classDef comp fill:#e8f1fb,stroke:#1168bd
  classDef iface fill:#fff7d6,stroke:#b8860b
  class RC,DC,AG,RS,DS,PS,SR,RP,SA,NA,IA comp
  class iSV,iRS,iDS,iPS,iSR,iRP,iFS,iNP,iID iface
```

## Key
| Notation | Meaning |
|---|---|
| Box inside the large frame | Component |
| Yellow circle | Interface (named contract) |
| Solid line component to circle | The component **provides** that interface |
| Dashed arrow component to circle | The component **requires** that interface |
| Solid arrow to cylinder or external box | Protocol used to reach an outside system |

## Notes
- Every external service (identity provider, file storage, email, database) is reached only through an adapter or repository component.
- `StatusRules` encodes the legal transitions in Diagram 7, so both services share one source of truth.
