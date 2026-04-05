---
name: spec-kit
description: |
  Guide spec-driven feature development using a structured three-phase workflow: Requirements → Design → Tasks.
  Use this skill whenever the user wants to plan a feature, write a spec, or do structured design before coding.
argument-hint: What feature are we planning?
---

# Spec-Driven Development

Turn a feature idea into a structured, reviewable spec before a single line of code is written. The workflow has three phases, each producing a file in `specs/<spec-name>/`. You move forward only when the user approves the current phase — this keeps the plan grounded in what they actually want.

```
specs/<spec-name>/
├── spec.md         ← Phase 0: Spec metadata (type, workflow)
├── requirements.md ← Phase 1: What to build
├── design.md       ← Phase 2: How to build it
└── tasks.md        ← Phase 3: Ordered implementation steps
```

## Phase 0: Spec

Before writing anything, nail down the spec name and create the folder.

1. If the user hasn't named the spec, propose a short slug (e.g., `user-auth`, `export-csv`).
2. Confirm with the user the spec name (and also the slug).
3. Add a number to the slug, it must be an increment of the last spec created inside `specs/` (e.g., `001`, `002`).
4. Create `specs/<spec-name>/` in the repo root. `<spec-name> = <number>-<spec-slug>` (e.g., `001-user-auth`, `002-export-csv`).

Read `./references/templates/spec.md` before writing this file.
  
Write `specs/<spec-name>/spec.md`. No details are required at this stage, just metadata.

**Note:** Use the YAML frontmatter to track spec type and workflow, currently `type: feature` and `workflow: requirements-first`.

## Phase 1: Requirements

**Goal:** Capture *what* the system must do, in unambiguous terms the user can validate.

Read `./references/templates/requirements.md` before writing this file.

Write `specs/<spec-name>/requirements.md` using EARS notation for user stories:

> `WHEN [condition/event] THE SYSTEM SHALL [expected behavior]`

Group stories by user-facing goal. For each story, list concrete acceptance criteria — the conditions a reviewer could check to say "done".

**Iterate:** Share the draft. Ask: "Does this capture everything? Anything missing or off?" Revise until approved, when approved update the YAML frontmatter, then move to Phase 2.

**Note:** Use the YAML frontmatter to track document status.

## Phase 2: Design

**Goal:** Decide *how* the system will fulfill the requirements — before writing code.

Read `./references/templates/design.md` before writing this file.

Write `specs/<spec-name>/design.md`. Every significant decision should trace back to a requirement. Use Mermaid for sequence/flow diagrams where it adds clarity; plain prose otherwise.

Cover:
- High-level architecture and component responsibilities
- Data models and storage
- Key interfaces / API contracts
- Critical flows as sequence diagrams
- Error handling and edge cases
- Testing strategy (what to unit-test vs. integrate-test)

**Iterate:** Share the draft. Ask: "Does this approach make sense? Any concerns?" Revise until approved, when approved update the YAML frontmatter, then move to Phase 3.

**Note:** Use the YAML frontmatter to track document status.

## Phase 3: Tasks

**Goal:** Break the design into discrete, ordered implementation steps a developer can pick up one at a time.

Read `./references/templates/tasks.md` before writing this file.

Write `specs/<spec-name>/tasks.md` as a checkbox list. Each task should:
- Be small enough to complete in one sitting
- Reference the requirement(s) it satisfies (e.g., `[REQ-2]`)
- Be ordered so earlier tasks unblock later ones (foundations first)

Avoid vague tasks like "implement auth". Prefer "Add `POST /auth/login` endpoint that validates credentials and returns a JWT `[REQ-1.2]`".

**Iterate:** Share the list. Ask: "Does the order feel right? Anything too big or missing?" Revise until approved, when approved update the YAML frontmatter.

**Note:** Use the YAML frontmatter to track document status.

# Re-entering the loop

The user may want to revisit an earlier phase — that's fine. If requirements change after the design is written, update `requirements.md` first, then revise `design.md` and `tasks.md` for consistency. The three files should always agree.

If the user drops in mid-workflow (e.g., they already have a `requirements.md`), read what exists, orient yourself, and pick up from the right phase.
