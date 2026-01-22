---
description: Refine a story into technical tasks through requirements analysis
---

# Story Refinement: $ARGUMENTS

## HARD CONSTRAINTS (NON-NEGOTIABLE)

- **NO CODE READING** — Do not read, analyze, or reference source code. Code analysis happens in `/analyze`.
- **NO SOLUTIONING** — Do not propose specific implementations, file changes, or architecture decisions.
- **NO IMPLEMENTATION DETAILS** — Tasks must describe WHAT needs to be done, never HOW to do it.
- **DO NOT CALL submit_plan** — This is a discussion, not a plan-and-implement cycle.

If you find yourself wanting to look at code, STOP. That belongs in the `/analyze` phase.

---

## Prerequisites

Fetch the story details in full (JSON preferred) before proceeding:
- For beads: `bd show $ARGUMENTS --json`

---

## 1. Story Description & AC Refinement

Before creating child tasks, ensure the story itself is well-defined:

| Check                   | Question                                                              |
| ----------------------- | --------------------------------------------------------------------- |
| **Clear User Value**        | Does the description explain who benefits and why?                    |
| **Scope Boundaries**        | Is it clear what is IN and OUT of scope for this story?               |
| **Testable AC**             | Can each acceptance criterion be verified without ambiguity?          |
| **Edge Cases**              | Are error states, empty states, and boundary conditions addressed?    |
| **Dependencies**            | Are blockers or prerequisites from other stories noted?               |

**Output:** List any gaps or ambiguities in the story that need clarification before proceeding.

---

## 2. Requirements Gap Analysis

Analyze the story through these lenses to identify what's missing from requirements (NOT solutions):

| Lens              | Key Questions                                                          |
| ----------------- | ---------------------------------------------------------------------- |
| **Completeness**      | Are all user-facing behaviors specified?                               |
| **Error Handling**    | What should happen when things go wrong? Are error messages defined?   |
| **Data Requirements** | What inputs are required? What outputs are expected?                   |
| **Security**          | Are there access control or validation requirements?                   |
| **Verification**      | How will we know this is done? What does "working" look like?          |

**Output:** Requirements Gap Report
- **Clarifications Needed:** [Questions for product/stakeholders]
- **Missing Requirements:** [Gaps in the story definition]
- **Edge Cases to Address:** [Boundary conditions not yet specified]

---

## 3. Propose Child Tasks

Break the story into discrete, deliverable tasks. Each task should be:
- **Outcome-focused** — Describes the result, not the implementation
- **Independently verifiable** — Has clear criteria for "done"
- **Small enough** — Can be analyzed and implemented in a single `/analyze` + `/implement` cycle

| Task Title | Goal                    | Verification Criteria      |
| ---------- | ----------------------- | -------------------------- |
| [Title]    | [What needs to be done] | [How to verify it's done]  |

---

## 4. Review & Create Tasks

1. **Present for confirmation** — Show the gap analysis and proposed tasks to the user
2. **Wait for explicit approval** — Do not create tasks until user confirms
3. **Create child tasks** in the project's work tracking system

**For projects using beads:**
```bash
bd add "<task title>" --parent=$ARGUMENTS
```
**MANDATORY for beads projects:** Every child task MUST include `--parent=$ARGUMENTS` to establish the hierarchy.

If tasks have dependencies on each other:
```bash
bd dep add <task-id> <blocked-by-id>
```

**For projects using other tracking systems (GitHub Issues, Jira, etc.):**
- Create tasks as children/sub-issues of the parent story
- Ensure parent-child relationship is established per that system's conventions

---

## Acceptance Criteria

- [ ] Story description and AC reviewed for clarity and completeness
- [ ] Gap analysis identifies missing requirements (not solutions)
- [ ] Child tasks are outcome-focused stubs (no implementation details)
- [ ] All tasks created with proper parent linkage (beads: `--parent=$ARGUMENTS`)
- [ ] No code was read or referenced during this process
