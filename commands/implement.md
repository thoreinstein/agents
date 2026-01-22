---
description: Execute an implementation plan with phase-gated commits
argument-hint: "<ticket-id> - must have a plan from /analyze"
---

# Implementation: $ARGUMENTS

## HARD CONSTRAINTS (NON-NEGOTIABLE)

- **SCOPE IS LOCKED** — Only implement what is in the plan. Nothing more.
- **NO SCOPE CREEP** — If you think "the user probably also wants X", STOP. If X is not in the plan, do not implement it.
- **TICKET TRACKING IS MANDATORY** — Every ticket worked must be updated (in-progress → complete).
- **EPIC CLOSURE RULES** — An epic is NOT complete until ALL child tickets are closed.

If you discover related work that should be done:
1. Note it as a follow-up item
2. Do NOT implement it
3. Continue with the scoped work

---

## Prerequisites

1. **Fetch the ticket details** (JSON preferred)
   - For beads: `bd show $ARGUMENTS --json`

2. **GUARD: No Acceptance Criteria → STOP**
   - If the ticket has no AC, inform the user:
     > "This ticket has no acceptance criteria. Run `/story <ticket-id>` or `/epic <ticket-id>` to refine before implementing."
   - Do not proceed.

3. **Load the implementation plan** from Obsidian: `working/plans/$ARGUMENTS-plan.md`
   - If no plan exists, inform the user:
     > "No implementation plan found. Run `/analyze $ARGUMENTS` first."
   - Do not proceed.

4. **GUARD: Epic Scope Check**
   - If this is an epic, count all child tickets

### Epic Scope Guard

| Condition | Action |
| --------- | ------ |
| **Single ticket (story/task)** | Proceed with implementation |
| **Epic with 1-3 children** | Proceed — manageable scope |
| **Epic with 4+ children** | **STOP** — Scope too large |

**If scope is too large:**
```
⚠️ SCOPE TOO LARGE FOR SINGLE IMPLEMENTATION RUN

This epic has [N] child tickets. Implementing all of them in one session increases risk of:
- Scope creep across ticket boundaries
- Missed ticket status updates
- Difficult-to-review commits

Recommendation: Run `/implement` on individual stories instead:
- [list child ticket IDs]

Would you like to proceed with a specific story, or continue with the full epic implementation anyway?
```

Wait for user confirmation before proceeding with large epics.

---

## Ticket Status Management

### For Beads-Tracked Projects

**When starting work on a ticket:**
```bash
bd update $TICKET_ID --status in-progress
```

**When completing a ticket:**
```bash
bd update $TICKET_ID --status done
```

**Verify epic closure prerequisites:**
```bash
bd show $EPIC_ID --json  # Check all children are status:done before closing epic
```

### Status Update Rules

| Event | Action |
| ----- | ------ |
| Starting implementation of $ARGUMENTS | `bd update $ARGUMENTS --status in-progress` |
| Starting a child ticket | `bd update <child-id> --status in-progress` |
| Completing a child ticket | `bd update <child-id> --status done` |
| All children complete → close epic | `bd update $ARGUMENTS --status done` |

**CRITICAL:** An epic can ONLY be marked done when ALL child tickets are done. Verify before closing.

---

## Phase Execution Loop

Execute each phase from the implementation plan following this exact sequence:

```
Plan → Work → Verify → Commit → Update Tickets → Proceed
```

### For Each Phase:

#### 1. Plan
- Review the phase scope from the implementation plan
- Identify the specific tickets/tasks in this phase
- **Mark each ticket as in-progress** in the tracking system

#### 2. Work
- Execute ONLY the implementation work defined in the plan
- Follow existing codebase patterns
- Delegate to specialists as needed

**SCOPE CHECK:** Before writing any code, verify it's in the plan. If not, don't write it.

#### 3. Verify
- Run tests relevant to the changes
- Run lints and type checks
- Verify acceptance criteria are met
- Ensure no regressions

#### 4. Commit
- Invoke `/commit` to stage and commit the phase work
- Include ticket ID(s) in commit message for traceability

```
⚠️  DO NOT PROCEED TO NEXT PHASE UNTIL COMMIT SUCCEEDS
```

If commit fails, fix issues and retry. Never advance with uncommitted work.

#### 5. Update Tickets
- **Mark completed tickets as done** in the tracking system
- For beads: `bd update <ticket-id> --status done`

#### 6. Proceed
- Verify all phase tickets are marked complete
- Move to the next phase

---

## Scope Enforcement

### Before Writing Any Code, Ask:

1. Is this work item in the implementation plan? → If NO, do not implement
2. Is this ticket in scope for the current phase? → If NO, do not implement
3. Am I about to implement something "nice to have"? → If YES, note it as follow-up, do not implement

### When You Encounter Related Work

```markdown
## Discovered Follow-Up Items

| Item | Related Ticket | Why Not In Scope |
| ---- | -------------- | ---------------- |
| [description] | [ticket-id or "new"] | Not in plan for $ARGUMENTS |
```

Add to the implementation summary, but DO NOT implement.

---

## Epic Completion Checklist

Before marking an epic as complete:

```
┌─────────────────────────────────────────────────────────────┐
│  EPIC COMPLETION GATE                                       │
├─────────────────────────────────────────────────────────────┤
│  [ ] All child tickets are status: done                     │
│  [ ] All phases in the plan are complete                    │
│  [ ] All commits are pushed (if applicable)                 │
│  [ ] No uncommitted changes remain                          │
│  [ ] Implementation summary documented                      │
│                                                             │
│  ⚠️  DO NOT CLOSE EPIC IF ANY CHILD IS STILL OPEN           │
└─────────────────────────────────────────────────────────────┘
```

**Verify with:**
```bash
bd show $ARGUMENTS --json  # Check children array, all should be status:done
```

---

## Final Cleanup

After the last phase:

1. Verify all child tickets are marked complete
2. Confirm no uncommitted changes remain
3. **Only then** mark the epic/parent ticket as complete
4. Document any follow-up items discovered during implementation

---

## Traceability

Throughout implementation:
- Keep ticket ID visible in reasoning and summaries
- Include ticket ID in commit messages and branch names
- Update ticket status in real-time, not at the end
- If you discover out-of-scope work, note it as follow-up — DO NOT EXPAND SCOPE

---

## Begin Implementation

Now implement ticket: **$ARGUMENTS**

Start by:
1. Fetching the ticket details
2. Loading the plan from `working/plans/$ARGUMENTS-plan.md`
3. Marking the ticket as in-progress
4. Beginning Phase 1

**Remember: The plan is the scope. Do not exceed it.**
