# Diagram 8 – UML Package: Planned folder structure of the PrintAhead codebase

**Scope:** the folders we will create under the project root and the allowed dependencies between them.
**Primary audience:** the developers (and the reviewer of every pull request).
**Risk it reduces:** tangled code, such as pages that run SQL directly or business rules copied into UI components.

**Layering rule:** Pages and components call only services, and only repositories touch the database and only adapters touch external services.

```mermaid
---
title: "Package diagram: planned folder structure of the PrintAhead codebase"
---
flowchart TB
  subgraph ROOT["Project root"]
    direction TB
    subgraph SRC["src/"]
      direction TB
      APP["app/<br/>pages for student, staff, runner<br/>and API route handlers"]
      COMP["components/<br/>reusable UI pieces"]
      SVC["services/<br/>request, pricing, delivery,<br/>and status rules"]
      REPO["repositories/<br/>all SQL access"]
      ADP["adapters/<br/>identity, file storage, email"]
      DOM["domain/<br/>types and enums"]
    end
    DBP["db/<br/>schema and migrations"]
  end

  style ROOT fill:#ffffff,stroke:#444444
  style SRC fill:#f4f8fd,stroke:#1168bd

  APP -.->|uses| COMP
  APP -.->|calls| SVC
  COMP -.->|uses types from| DOM
  SVC -.->|reads and writes through| REPO
  SVC -.->|talks to outside through| ADP
  SVC -.->|uses types from| DOM
  REPO -.->|uses types from| DOM
  REPO -.->|runs against| DBP
  ADP -.->|uses types from| DOM
```

## Key
| Notation | Meaning |
|---|---|
| Box | Package (a folder) with a one-line responsibility |
| Dashed arrow | Dependency: the source folder may import from the target folder |
| No arrow | Importing is not allowed (for example `app/` to `repositories/`) |

## Notes
- Folder names are proposed; adjust them to match what you actually create in the repository.
