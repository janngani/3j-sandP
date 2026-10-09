# Diagram 2 – C4 Container: SorSU-Bulan PrintAhead

**Scope:** every deployable or runnable unit inside the system, with its technology.
**Primary audience:** the development team and the instructor.
**Risk it reduces:** unclear technology choices and unclear ownership of data and files.

**Next.js decision (one sentence):** We drew the Next.js app as **two containers**, the Web Application (UI that runs in the user's browser) and the API Application (route handlers that run on the server), because they run in different places even though they ship from one Next.js build.

```mermaid
---
title: "Container diagram for SorSU-Bulan PrintAhead"
---
flowchart TB
  student["<b>Student</b><br/>[Person]<br/>Submits print requests and tracks them"]
  staff["<b>Print Shop Staff</b><br/>[Person]<br/>Reviews, prices, and prints requests"]
  runner["<b>Student Runner</b><br/>[Person]<br/>Delivers printouts and collects cash"]

  subgraph SYS["SorSU-Bulan PrintAhead [Software System boundary]"]
    direction TB
    web["<b>Web Application</b><br/>[Container: Next.js, React, runs in browser]<br/>Request form, status pages, staff queue, and runner job list"]
    api["<b>API Application</b><br/>[Container: Next.js route handlers, Node.js]<br/>Authentication, validation, request workflow, pricing, and notifications"]
    db[("<b>Database</b><br/>[Container: PostgreSQL]<br/>Users, requests, file records, deliveries, payments, status history")]
    files[("<b>File Storage</b><br/>[Container: S3-compatible object storage]<br/>Uploaded DOCX, PDF, and XLSX files")]
  end

  idp["<b>SorSU Email Identity Provider</b><br/>[External System]<br/>Confirms SorSU email accounts"]
  mail["<b>Email Notification Service</b><br/>[External System]<br/>Sends status emails"]

  student -->|"Fills in the request form and tracks status<br/>[HTTPS]"| web
  staff -->|"Reviews, prices, and prints requests<br/>[HTTPS]"| web
  runner -->|"Accepts and completes delivery jobs<br/>[HTTPS]"| web
  web -->|"Sends requests and reads request status<br/>[HTTPS/JSON]"| api
  web -->|"Sends the user to sign in with a SorSU email<br/>[HTTPS, OIDC redirect]"| idp
  api -->|"Verifies the sign-in result<br/>[HTTPS, OIDC]"| idp
  api -->|"Reads and writes records<br/>[SQL over TLS]"| db
  api -->|"Stores and fetches uploaded files<br/>[HTTPS, S3 API]"| files
  api -->|"Sends status emails<br/>[HTTPS, email API]"| mail

  classDef person fill:#08427b,stroke:#052e56,color:#ffffff
  style SYS fill:#ffffff,stroke:#444444,stroke-dasharray: 6 4
  classDef cont fill:#438dd5,stroke:#2e6295,color:#ffffff
  classDef ext fill:#999999,stroke:#6b6b6b,color:#ffffff
  class student,staff,runner person
  class web,api,db,files cont
  class idp,mail ext
```

## Key
| Element | Meaning |
|---|---|
| Container (light blue box) | A separately running unit; the second line is its technology |
| Cylinder | A data store |
| External system (grey box) | A system we depend on but do not build |
| Arrow | Intent of the call, followed by the protocol in brackets |

## Notes
- The technology choices (Next.js, PostgreSQL, S3-compatible storage) are **provisional** until confirmed in your Activity 5 architecture decision.
