# Diagram 6 – UML Class: PrintAhead domain model

**Scope:** 6 domain classes and 4 enumerations of the MVP.
**Primary audience:** developers and the database designer.
**Risk it reduces:** inconsistent names and rules for the same business concept across code, database, and screens.

```mermaid
---
title: "Class diagram: PrintAhead domain model"
---
classDiagram
  class User {
    +UUID id
    +String fullName
    +String email
    +UserRole role
  }
  class PrintRequest {
    +UUID id
    +String requesterName
    +String deliveryRoom
    +String instructions
    +DateTime submittedAt
    +RequestStatus status
  }
  class PrintFile {
    +UUID id
    +String originalName
    +FileType fileType
    +Integer sizeBytes
    +String storageKey
    +DateTime uploadedAt
  }
  class Delivery {
    +UUID id
    +DateTime acceptedAt
    +DateTime deliveredAt
    +String failureNote
  }
  class Payment {
    +UUID id
    +Decimal printCost
    +Decimal serviceFee
    +Decimal totalAmount
    +PaymentStatus status
    +DateTime collectedAt
  }
  class StatusChange {
    +UUID id
    +RequestStatus fromStatus
    +RequestStatus toStatus
    +DateTime changedAt
  }
  class UserRole {
    <<enumeration>>
    Student
    PrintShopStaff
    Runner
  }
  class RequestStatus {
    <<enumeration>>
    Submitted
    Accepted
    Rejected
    Printing
    ReadyForDelivery
    OutForDelivery
    Completed
    Cancelled
  }
  class PaymentStatus {
    <<enumeration>>
    Pending
    Collected
  }
  class FileType {
    <<enumeration>>
    DOCX
    PDF
    XLSX
  }

  User "1" --> "0..*" PrintRequest : submits
  PrintRequest "1" *-- "1..*" PrintFile : contains
  PrintRequest "1" --> "0..*" Delivery : delivered through
  User "1" --> "0..*" Delivery : performs as runner
  PrintRequest "1" *-- "0..1" Payment : billed by
  PrintRequest "1" *-- "1..*" StatusChange : records
  User "1" --> "0..*" StatusChange : makes
```

## Key
| Notation | Meaning |
|---|---|
| `+Type name` | Public attribute with its type |
| `<<enumeration>>` | Fixed set of allowed values |
| Open arrow | Association (the source knows the target) |
| Filled diamond | Composition: the part cannot exist without the whole |
| Numbers at line ends | Multiplicity (`1`, `0..1`, `0..*`, `1..*`) |

## Notes
- `RequestStatus` values are copied exactly from the states in Diagram 7.
- `PrintRequest` has no Payment until staff sets the price, hence `0..1`.
- A request can have more than one Delivery if the student is absent and the job is re-queued.
- `requesterName` is the full name typed in the form; it can differ from `User.fullName`.
