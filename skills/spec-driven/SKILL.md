---
name: spec-driven
description: |
  Guide spec-driven feature development using a structured three-phase workflow: requirements → design → tasks.
  Use this skill whenever the user wants to plan a feature, write a spec, or do structured design before coding.
  Trigger on phrases like "let's spec this out", "I want to plan this properly", "write a PRD", "help me think through this feature", or "break this into tasks".
---

# Spec-Driven Development

Turn a feature idea into a structured, reviewable spec before a single line of code is written. The workflow has three phases, each producing a file in `specs/<feature-name>/`. You move forward only when the user approves the current phase — this keeps the plan grounded in what they actually want.

```
specs/<feature-name>/
├── requirements.md   ← Phase 1: what to build
├── design.md         ← Phase 2: how to build it
└── tasks.md          ← Phase 3: ordered implementation steps
```

---

## Phase 0: Init

Before writing anything, nail down the feature name and create the folder.

1. If the user hasn't named the feature, propose a short slug (e.g., `user-auth`, `export-csv`). Confirm with them.
2. Create `specs/<feature-name>/` in the repo root.
3. Tell the user which phase you're starting and what you'll produce.

---

## Phase 1: Requirements

**Goal:** Capture *what* the system must do, in unambiguous terms the user can validate.

Read `references/templates/requirements.md` before writing this file.

Write `specs/<feature-name>/requirements.md` using EARS notation for user stories:

> `WHEN [condition/event] THE SYSTEM SHALL [expected behavior]`

Group stories by user-facing goal. For each story, list concrete acceptance criteria — the conditions a reviewer could check to say "done".

**Iterate:** Share the draft. Ask: "Does this capture everything? Anything missing or off?" Revise until the user approves, then move to Phase 2.

---

## Phase 2: Design

**Goal:** Decide *how* the system will fulfill the requirements — before writing code.

Read `references/templates/design.md` before writing this file.

Write `specs/<feature-name>/design.md`. Every significant decision should trace back to a requirement. Use Mermaid for sequence/flow diagrams where it adds clarity; plain prose otherwise.

Cover:
- High-level architecture and component responsibilities
- Data models and storage
- Key interfaces / API contracts
- Critical flows as sequence diagrams
- Error handling and edge cases
- Testing strategy (what to unit-test vs. integrate-test)

**Iterate:** Share the draft. Ask: "Does this approach make sense? Any concerns?" Revise until approved, then move to Phase 3.

---

## Phase 3: Tasks

**Goal:** Break the design into discrete, ordered implementation steps a developer can pick up one at a time.

Read `references/templates/tasks.md` before writing this file.

Write `specs/<feature-name>/tasks.md` as a checkbox list. Each task should:
- Be small enough to complete in one sitting
- Reference the requirement(s) it satisfies (e.g., `[REQ-2]`)
- Be ordered so earlier tasks unblock later ones (foundations first)

Avoid vague tasks like "implement auth". Prefer "Add `POST /auth/login` endpoint that validates credentials and returns a JWT `[REQ-1.2]`".

**Iterate:** Share the list. Ask: "Does the order feel right? Anything too big or missing?" Revise until approved.

---

## Re-entering the loop

The user may want to revisit an earlier phase — that's fine. If requirements change after the design is written, update `requirements.md` first, then revise `design.md` and `tasks.md` for consistency. The three files should always agree.

If the user drops in mid-workflow (e.g., they already have a `requirements.md`), read what exists, orient yourself, and pick up from the right phase.

---

## References

- `references/templates/requirements.md` — requirements file template (read at Phase 1)
- `references/templates/design.md` — design file template (read at Phase 2)
- `references/templates/tasks.md` — tasks file template (read at Phase 3)
