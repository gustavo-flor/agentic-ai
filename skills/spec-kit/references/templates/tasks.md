# Tasks: [Spec Name]

Implementation tasks derived from the design. Complete them in order — each task should leave the codebase in a working state.

## Setup & Foundation

- [ ] **Task 1:** [e.g., Create database migration for `users` table with fields: id, email, password_hash, created_at] `[REQ-1.1]`
- [ ] **Task 2:** [e.g., Add `User` model with validation and password hashing methods] `[REQ-1.1]`

## Core Implementation

- [ ] **Task 3:** [e.g., Implement `POST /auth/login` endpoint — validate credentials, return JWT on success, 401 on failure] `[REQ-1.1, REQ-1.2]`
- [ ] **Task 4:** [e.g., Add login attempt tracking and lockout logic after N failures] `[REQ-1.2]`
- [ ] **Task 5:** [e.g., Implement `POST /auth/reset-request` — generate time-limited reset token and send email] `[REQ-2.1]`
- [ ] **Task 6:** [e.g., Implement `POST /auth/reset-confirm` — validate token, set new password, invalidate token] `[REQ-2.2]`

## Error Handling & Edge Cases

- [ ] **Task 7:** [e.g., Add global error handler for 500s with structured logging]
- [ ] **Task 8:** [e.g., Handle expired/reused reset tokens with clear user-facing error]

## Tests

- [ ] **Task 9:** [e.g., Unit tests for password validation and hashing] `[REQ-1.1]`
- [ ] **Task 10:** [e.g., Integration tests for full login and reset flows] `[REQ-1.1, REQ-2.1, REQ-2.2]`

## Cleanup

- [ ] **Task 11:** [e.g., Update API docs / OpenAPI spec]
- [ ] **Task 12:** [e.g., Add feature flag if rolling out incrementally]

_Tasks reference requirements with `[REQ-N.N]` notation. See `requirements.md` for the full list._
