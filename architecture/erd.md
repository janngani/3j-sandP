# Diagram 11 – Draft ERD: PrintAhead database

**Scope:** one table per stored class in Diagram 6, with keys, cardinality, and personal data (PII) marked.
**Primary audience:** the database designer, developers, and the data-privacy reviewer.
**Risk it reduces:** missing relationships, and personal data stored or exposed without anyone having decided to.

```mermaid
---
title: "Draft ERD: PrintAhead database"
---
erDiagram
  users ||--o{ print_requests : submits
  print_requests ||--|{ print_files : contains
  print_requests ||--o{ deliveries : "delivered through"
  users ||--o{ deliveries : "performs as runner"
  print_requests ||--o| payments : "billed by"
  print_requests ||--|{ status_changes : records
  users ||--o{ status_changes : makes

  users {
    uuid id PK
    string full_name "PII"
    string email UK "PII"
    string role "UserRole"
    timestamp created_at
  }
  print_requests {
    uuid id PK
    uuid requester_id FK
    string requester_name "PII"
    string delivery_room "PII"
    string instructions
    string status "RequestStatus"
    timestamp submitted_at
  }
  print_files {
    uuid id PK
    uuid request_id FK
    string original_name "PII"
    string file_type "FileType"
    int size_bytes
    string storage_key
    timestamp uploaded_at
  }
  deliveries {
    uuid id PK
    uuid request_id FK
    uuid runner_id FK
    timestamp accepted_at
    timestamp delivered_at
    string failure_note
  }
  payments {
    uuid id PK
    uuid request_id FK, UK
    decimal print_cost
    decimal service_fee
    decimal total_amount
    string status "PaymentStatus"
    timestamp collected_at
  }
  status_changes {
    uuid id PK
    uuid request_id FK
    uuid changed_by FK
    string from_status "RequestStatus"
    string to_status "RequestStatus"
    timestamp changed_at
  }
```

## Key
| Notation | Meaning |
|---|---|
| PK / FK / UK | Primary key / foreign key / unique key |
| `"PII"` | Column holding personally identifiable information |
| `"RequestStatus"` etc. | Column stores the names from that enumeration in Diagram 6 |
| `\|\|--o{` | Exactly one to zero-or-many (crow's foot) |
| `\|\|--\|{` | Exactly one to one-or-many |
| `\|\|--o\|` | Exactly one to zero-or-one |

## PII notes
- `users.full_name`, `users.email`, `print_requests.requester_name`, `print_requests.delivery_room`, and `print_files.original_name` identify a person or where they are.
- The **contents** of uploaded files may also contain personal data; decide how long files are kept (for example, delete after completion).
