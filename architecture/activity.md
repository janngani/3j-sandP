# Diagram 4 – UML Activity: Print request from submission to cash-on-delivery

**Scope:** the core workflow, one swimlane per role (Student, Print Shop Staff, Student Runner) and one for the System.
**Primary audience:** the print shop owner and the team (to agree on who does what, and when).
**Risk it reduces:** handoffs nobody owns, such as who tells the runner a job is ready, or what happens when the student is not in the room.

```mermaid
---
title: "Activity diagram: print request from submission to cash on delivery (4 swimlanes)"
---
flowchart TB
  subgraph STU["Swimlane: Student"]
    direction TB
    s0((Start)) --> s1["Sign in with SorSU email"]
    s1 --> s2["Fill form: full name, room, attach DOCX/PDF/XLSX, add instructions"]
    s2 --> s3["Submit request"]
    s9["Receive printouts and pay cash"]
  end

  subgraph SYS["Swimlane: System"]
    direction TB
    y1{"Form and files valid?"}
    y2["Save request as Submitted; email staff"]
    y3["Set Rejected; email student the reason"]
    y4["Set Accepted; email student the price"]
    y5["Set Printing"]
    y6["Set ReadyForDelivery; email runners"]
    y7["Set OutForDelivery; email student"]
    y8["Set Completed; email student"]
    y9["Set ReadyForDelivery again; email runners"]
    e1(((End)))
  end

  subgraph STF["Swimlane: Print Shop Staff"]
    direction TB
    f1{"Request acceptable?"}
    f2["Enter price for printing"]
    f3["Start printing"]
    f4["Print documents and mark ready"]
  end

  subgraph RUN["Swimlane: Student Runner"]
    direction TB
    r1["Accept delivery job"]
    r2["Collect printouts at the shop"]
    r3["Go to the student's room"]
    r4{"Student in the room?"}
    r5["Hand over printouts and collect cash"]
    r6["Confirm cash received"]
    r7["Return printouts to the shop"]
  end

  style STU fill:#e8f1fb,stroke:#1168bd
  style SYS fill:#f1f1f1,stroke:#777777
  style STF fill:#e9f6ec,stroke:#2e8b57
  style RUN fill:#fff1e0,stroke:#d9822b

  s3 --> y1
  y1 -->|"[invalid]"| s2
  y1 -->|"[valid]"| y2
  y2 --> f1
  f1 -->|"[rejected]"| y3
  y3 --> e1
  f1 -->|"[accepted]"| f2
  f2 --> y4
  y4 --> f3
  f3 --> y5
  y5 --> f4
  f4 --> y6
  y6 --> r1
  r1 --> y7
  y7 --> r2
  r2 --> r3
  r3 --> r4
  r4 -->|"[present]"| r5
  r5 --> s9
  r5 --> r6
  r6 --> y8
  y8 --> e1
  r4 -->|"[absent]"| r7
  r7 --> y9
  y9 --> r1
```

## Key
| Shape | Meaning |
|---|---|
| Small circle "Start" | Initial node |
| Double circle "End" | Final node |
| Rectangle | Action, placed in the swimlane of whoever does it |
| Diamond | Decision; every outgoing arrow carries a guard in `[brackets]` |
| Coloured box | Swimlane (Student blue, System grey, Print Shop Staff green, Student Runner orange) |

## Notes
- Mermaid cannot force straight lanes, so each coloured box is one lane and the position on the page is chosen automatically.
- Status names in "Set ..." actions match the `RequestStatus` enumeration in Diagram 6 and the states in Diagram 7.
- Student cancellation (Submitted or Accepted only) is a separate flow shown in Diagram 7.
