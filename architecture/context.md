# Diagram 1 – C4 System Context: SorSU-Bulan PrintAhead

**Scope:** the whole system as one box, every user role, and every external system.
**Primary audience:** instructor, print shop owner, and anyone new to the project (non-technical).
**Risk it reduces:** building for the wrong boundary, or forgetting a user role or outside system the MVP depends on.

```mermaid
---
title: "System Context diagram for SorSU-Bulan PrintAhead (online print request with room delivery)"
---
flowchart TB
  student["<b>Student</b><br/>[Person]<br/>SorSU-Bulan student who needs school documents printed and brought to their room"]
  staff["<b>Print Shop Staff</b><br/>[Person]<br/>Reviews requests, sets the price, and prints the documents"]
  runner["<b>Student Runner</b><br/>[Person]<br/>Collects printouts at the shop, delivers them to the room, and collects cash"]

  pa["<b>SorSU-Bulan PrintAhead</b><br/>[Software System]<br/>Web system where students submit print requests online and receive printouts in their room, paid in cash on delivery"]

  idp["<b>SorSU Email Identity Provider</b><br/>[External System]<br/>Confirms that a person owns a valid SorSU email account"]
  mail["<b>Email Notification Service</b><br/>[External System]<br/>Delivers status emails to students, staff, and runners"]

  student -->|"Submits print requests (files, room, instructions) and tracks their status"| pa
  staff -->|"Reviews requests, sets the price, prints, and marks documents ready"| pa
  runner -->|"Accepts delivery jobs, delivers printouts, and confirms cash received"| pa
  pa -->|"Verifies the user's SorSU email sign-in"| idp
  pa -->|"Sends request and delivery status emails"| mail

  classDef person fill:#08427b,stroke:#052e56,color:#ffffff
  classDef system fill:#1168bd,stroke:#0b4884,color:#ffffff
  classDef ext fill:#999999,stroke:#6b6b6b,color:#ffffff
  class student,staff,runner person
  class pa system
  class idp,mail ext
```

## Key
| Element | Meaning |
|---|---|
| Person (dark blue box) | A human user role of the system |
| Software system (blue box) | The system we are building (drawn as one box) |
| External system (grey box) | A system we depend on but do not build |
| Arrow | A relationship; the label states what the sender is trying to do |

## Notes
- Payment is **cash on delivery**, so there is no payment provider in this diagram.
- Every actor here appears in the use case diagram (Diagram 3).
- Drawn with a Mermaid flowchart styled to C4 notation (name, element type, description) because Mermaid's native `C4Context` syntax gives poor layout control.
