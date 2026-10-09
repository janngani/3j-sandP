# Diagram 7 – UML State Machine: PrintRequest life cycle

**Scope:** the main entity, `PrintRequest`, from submission to a final state.
**Primary audience:** developers (to enforce legal status changes) and the print shop (to understand each status).
**Risk it reduces:** illegal status jumps, such as delivering a request that was never printed or cancelling after printing has started.

```mermaid
---
title: "State machine diagram: PrintRequest life cycle"
---
stateDiagram-v2
  [*] --> Submitted : student submits request
  Submitted --> Accepted : staff accepts and sets price
  Submitted --> Rejected : staff rejects request
  Submitted --> Cancelled : student cancels
  Accepted --> Cancelled : student cancels before printing starts
  Accepted --> Printing : staff starts printing
  Printing --> ReadyForDelivery : staff marks printouts ready
  ReadyForDelivery --> OutForDelivery : runner accepts delivery job
  OutForDelivery --> Completed : runner confirms cash received
  OutForDelivery --> ReadyForDelivery : runner reports student absent
  Rejected --> [*]
  Cancelled --> [*]
  Completed --> [*]
```

## Key
| Notation | Meaning |
|---|---|
| Filled circle `[*]` (start) | Initial state |
| Circle `[*]` (end) | Final state |
| Rounded box | State, named as a condition the request is in |
| Arrow label | The event that causes the transition |

## State names versus the class diagram
These eight names are identical to the values of the `RequestStatus` enumeration in `class.md`:
`Submitted`, `Accepted`, `Rejected`, `Printing`, `ReadyForDelivery`, `OutForDelivery`, `Completed`, `Cancelled`.
