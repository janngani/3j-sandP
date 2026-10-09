# Architecture Design Document: SorSU-Bulan PrintAhead (Activity 7)

**Team:** 3Js and P  **Block:** BSIS 4-2
**System in one sentence:** A student signs in with a SorSU email, fills in a request form (full name, current room, attached DOCX/PDF/XLSX files, and a message with printing instructions), the print shop prints it, and a student runner delivers the printouts to the student's room, with payment in cash on delivery.

## Diagram index

| # | Diagram | File | Owner | Reviewer |
|---|---|---|---|---|
| 1 | C4 system context | [context.md](context.md) | Member A | |
| 2 | C4 container | [containers.md](containers.md) | Member A | |
| 3 | Use case | [use-cases.md](use-cases.md) | Member B | |
| 4 | Activity | [activity.md](activity.md) | Member B | |
| 5 | Sequence | [sequence.md](sequence.md) | Member C | |
| 6 | Class | [class.md](class.md) | Member D | |
| 7 | State machine | [state-machine.md](state-machine.md) | Member D | |
| 8 | Package | [packages.md](packages.md) | Member C | |
| 9 | UML component | [components.md](components.md) | Member C | |
| 10 | Deployment (Provisional) | [deployment.md](deployment.md) | Member A | |
| 11 | ERD (draft) | [erd.md](erd.md) | Member D | |

> The owners above follow the suggested 4-member split in the activity sheet. Replace "Member A to D" with real names and fill in the reviewer column before submitting.

## Cross-view consistency checks (Section 16)

| Check | Result |
|---|---|
| Every actor in Diagram 1 appears in Diagram 3 (Student, Print Shop Staff, Student Runner, SorSU Email Identity Provider, Email Notification Service) | Yes |
| Every container in Diagram 2 appears in Diagram 10 (Web App, API App, Database, File Storage) | Yes |
| The API Application in Diagram 9 is the same container as in Diagram 2, and its external services match Diagrams 1 and 2 | Yes |
| `RequestStatus` enumeration in Diagram 6 equals the state names in Diagram 7 (8 values) | Yes |
| Every status written in Diagram 4 ("Set Accepted", "Set Printing", ...) is a state in Diagram 7 | Yes |
| Every class in Diagram 6 that is stored has a table in Diagram 11, and multiplicities match crow's-foot cardinalities | Yes |
| Layering rule in Diagram 8 matches the components in Diagram 9 (services use repositories and adapters; controllers use services) | Yes |
| Every use case in Diagram 3 appears in a workflow step of Diagram 4 or in a state transition of Diagram 7 | Yes, except Track Request Status and Receive Status Notification, which read the status changes recorded by the other steps |

## Assumptions to confirm before approval

1. **Technology** (Next.js, PostgreSQL, S3-compatible storage, email API) is assumed. Replace it with your Activity 5 decision if it differs.
2. **Sign-in** uses an OIDC identity provider restricted to SorSU email addresses. Confirm which provider SorSU uses, and how print shop staff and runners get their roles (for example, an allow-list kept by an administrator).
3. **Notifications** are by email only. SMS or in-app push is not drawn.
4. **Pricing:** staff set the print price per request, plus the proposed fixed service fee from the Lean Canvas. Delivery fee amounts are not yet validated.
5. **Failed delivery:** if the student is not in the room, the runner returns the printouts and the job is re-queued. Confirm this with the print shop.
6. **File retention:** how long uploaded files are kept is not decided.

## Contribution log and AI-use disclosure (draft, each member edits their own row)

| Member | Work done (drafted / reviewed) | Commit evidence | AI tools used, and how the output was checked |
|---|---|---|---|
| | | | Claude (Anthropic) was used to draft the Mermaid code for all 11 diagrams from our description and Activities 2 to 4. We checked every actor, status, class, table, and flow against our own feature list and the consistency table above, and corrected: _(write what you changed)_ |
