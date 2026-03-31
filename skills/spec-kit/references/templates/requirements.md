# Requirements: [Spec Name]

## Overview

[One paragraph describing the feature, its purpose, and who it's for.]

## User Stories

### [Goal 1: e.g., "User can log in"]

**REQ-1.1** WHEN a registered user submits valid credentials THE SYSTEM SHALL authenticate them and return a session token.

**REQ-1.2** WHEN a user submits invalid credentials THE SYSTEM SHALL reject the request and display a clear error message.

**Acceptance criteria:**
- [ ] Valid credentials result in a session token with a configurable expiry
- [ ] Invalid credentials return HTTP 401 with a human-readable message
- [ ] Repeated failed attempts trigger a lockout after N tries

### [Goal 2: e.g., "User can reset their password"]

**REQ-2.1** WHEN a user requests a password reset THE SYSTEM SHALL send a time-limited reset link to their registered email.

**REQ-2.2** WHEN a user follows a valid reset link THE SYSTEM SHALL allow them to set a new password.

**Acceptance criteria:**
- [ ] Reset links expire after 1 hour
- [ ] Used reset links cannot be reused
- [ ] Password must meet minimum complexity requirements

## Out of Scope

- [Things explicitly not covered by this spec]
- [Related features deferred to a future spec]
