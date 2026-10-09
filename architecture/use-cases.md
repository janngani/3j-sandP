# Diagram 3 – UML Use Case: SorSU-Bulan PrintAhead MVP

**Scope:** the 11 goal-level use cases of the MVP and the five actors from Diagram 1.
**Primary audience:** the instructor, the print shop owner, and the team (to agree on scope).
**Risk it reduces:** building features nobody asked for, or missing a feature a role needs.

```mermaid
---
title: "Use Case diagram: SorSU-Bulan PrintAhead MVP"
config:
  flowchart:
    defaultRenderer: elk
---
flowchart LR
  student(["Student"])
  idp(["SorSU Email Identity Provider<br/>(external)"])
  mail(["Email Notification Service<br/>(external)"])
  staff(["Print Shop Staff"])
  runner(["Student Runner"])

  subgraph SYS["System boundary: SorSU-Bulan PrintAhead"]
    direction TB
    UC2(["Submit Print Request"])
    UC3(["Track Request Status"])
    UC4(["Cancel Print Request"])
    UC1(["Sign In with SorSU Email"])
    UC5(["Receive Status Notification"])
    UC6(["Review Print Request"])
    UC7(["Set Print Price"])
    UC8(["Print Documents"])
    UC9(["Accept Delivery Job"])
    UC10(["Deliver Printouts"])
    UC11(["Confirm Cash Payment"])
  end

  student --- UC1
  student --- UC2
  student --- UC3
  student --- UC4
  student --- UC5
  idp --- UC1
  mail --- UC5
  UC1 --- staff
  UC5 --- staff
  UC6 --- staff
  UC7 --- staff
  UC8 --- staff
  UC1 --- runner
  UC5 --- runner
  UC9 --- runner
  UC10 --- runner
  UC11 --- runner

  style SYS fill:#ffffff,stroke:#444444
```

## Key
| Shape / line | Meaning |
|---|---|
| Rounded box outside the boundary | Actor (person or external system) |
| Oval inside the boundary | Use case, named verb + object |
| Plain line | Association: the actor takes part in the use case |

## Traceability: MVP feature to use case
| MVP feature (from your description) | Use case |
|---|---|
| Log in with a SorSU email account | Sign In with SorSU Email |
| Request form: full name, room, attached files, instruction message | Submit Print Request |
| See where the request is in the process | Track Request Status |
| Withdraw a request before printing starts | Cancel Print Request |
| Email updates when status changes | Receive Status Notification |
| Shop checks the files and instructions | Review Print Request |
| Shop sets the amount for cash payment | Set Print Price |
| Shop prints and marks the job ready | Print Documents |
| Runner takes a ready job | Accept Delivery Job |
| Runner brings printouts to the room | Deliver Printouts |
| Runner collects cash and closes the job | Confirm Cash Payment |

> Replace this table with your own MVP feature list if it differs. Every row here must match a Must-have feature.
