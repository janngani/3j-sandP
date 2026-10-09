# Diagram 5 – UML Sequence: Student submits a print request with file upload

**Scope:** the riskiest flow, which combines login state, file upload to storage, a database write, and an external email service.
**Primary audience:** the developers building the form, API, and storage code.
**Risk it reduces:** half-saved requests (files stored but no record, or the reverse), and an email failure blocking a valid request.

Sign-in with the SorSU email provider happens **before** this flow; this diagram starts with the student already holding a session.

```mermaid
---
title: "Sequence diagram: student submits a print request with file upload"
---
sequenceDiagram
  autonumber
  actor S as Student
  participant W as Web Application (Next.js UI)
  participant A as API Application
  participant D as Database (PostgreSQL)
  participant F as File Storage
  participant E as Email Notification Service (external)

  S->>W: Fill form (full name, room, files, instructions) and press Submit
  W->>A: POST /api/requests (multipart, session cookie)
  A->>A: Verify session and Student role
  alt Session missing or expired
    A-->>W: 401 Unauthorized
    W-->>S: Redirect to sign-in page
  else Session valid
    A->>A: Validate fields, and check each file is DOCX, PDF, or XLSX within the size limit
    alt Validation fails
      A-->>W: 422 with field errors
      W-->>S: Show errors and keep the entered values
    else Validation passes
      loop For each attached file
        A->>F: Store file (HTTPS)
        F-->>A: Storage key
      end
      alt File Storage unavailable
        A-->>W: 503 Service unavailable
        W-->>S: Show "Upload failed, please try again"
      else All files stored
        A->>D: Insert request, file records, and Submitted status change (one transaction)
        alt Database error
          D-->>A: Error
          A-)F: Delete the files just stored (cleanup)
          A-->>W: 500 Server error
          W-->>S: Show "Could not save, please try again"
        else Saved
          D-->>A: Saved, request id
          A-)E: Send "new request" email to staff and confirmation to student
          A-->>W: 201 Created with request id
          W-->>S: Show confirmation and the request status page
        end
      end
    end
  end
```

## Key
| Notation | Meaning |
|---|---|
| Solid arrow with filled head (`->>`) | Synchronous message |
| Dashed arrow (`-->>`) | Reply |
| Open arrowhead (`-)`) | Asynchronous message (no waiting for a reply) |
| `alt` / `else` | Alternative branches; only one runs |
| `loop` | Repeated for each file |

## Notes
- The email is asynchronous: if it fails, the request is still saved.
- `POST /api/requests` is a **proposed** route; align it with your actual repository.
