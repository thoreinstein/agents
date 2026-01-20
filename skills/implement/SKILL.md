---
name: implement
description: Full implementation mode - end-to-end feature implementation with phased execution, parallel work streams, verification gates, and atomic commits per phase
license: MIT
compatibility:
  - runtime:any
  - vcs:git
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
  - Edit
  - Bash
  - TodoWrite
  - TodoRead
metadata:
  author: thoreinstein
  version: 1.0.0
---

# Implement

Execute complete feature implementations using a structured, phased approach with verification gates and atomic commits.

## When to Use

- Building new features end-to-end
- Complex multi-component implementations
- Features requiring coordination across backend, frontend, and infrastructure
- Any implementation benefiting from structured phases and checkpoints

## Phase Execution Loop

Every phase follows this exact sequence:

```
┌─────────────────────────────────────────────────────────────┐
│  PHASE EXECUTION LOOP                                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   1. PLAN     → Define work for this phase                  │
│                 Mark phase as in_progress in task tracking  │
│                                                             │
│   2. WORK     → Execute the phase work                      │
│                 Delegate to appropriate specialists         │
│                                                             │
│   3. VERIFY   → Run verification checklist                  │
│                 Tests pass, lints clean, build succeeds     │
│                                                             │
│   4. COMMIT   → Create atomic commit for phase              │
│                 Mark phase as completed in task tracking    │
│                                                             │
│   5. PROCEED  → Only after commit succeeds                  │
│                 Move to next phase                          │
│                                                             │
│   ⚠️  DO NOT PROCEED TO NEXT PHASE UNTIL COMMIT SUCCEEDS    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Phase Overview

| Phase | Name | Description |
|-------|------|-------------|
| 1 | Requirements Analysis | Gather context, clarify requirements, set up tracking |
| 2 | Architecture Decision | Evaluate approaches, document decisions (if needed) |
| 3 | Parallel Implementation | Execute implementation across work streams |
| 4 | Integration | Integrate components, resolve conflicts |
| 5 | Verification | Run full verification checklist |
| 6 | Documentation & Cleanup | Update docs, clean up tracking, summarize |

See `references/implement-phases.md` for detailed phase instructions.

## Task Tracking Setup

Define ALL phases upfront before starting implementation:

```
Phase 1: Requirements Analysis
  - [ ] Gather context
  - [ ] Clarify requirements
  - [ ] Commit planning artifacts

Phase 2: Architecture Decision (if needed)
  - [ ] Evaluate approaches
  - [ ] Document decision
  - [ ] Commit architecture docs

Phase 3: Implementation
  - [ ] [Specific implementation tasks]
  - [ ] Write tests
  - [ ] Commit implementation

Phase 4: Integration
  - [ ] Integrate components
  - [ ] Commit integration

Phase 5: Verification
  - [ ] Run verification checklist
  - [ ] Commit fixes

Phase 6: Documentation & Cleanup
  - [ ] Update documentation
  - [ ] Create implementation summary
  - [ ] Final commit
```

## Verification Checklist

```
┌─────────────────────────────────────────────────────────────┐
│  VERIFICATION CHECKLIST                                     │
├─────────────────────────────────────────────────────────────┤
│  [ ] Static analysis clean on all changed files             │
│  [ ] All tests pass (unit, integration, e2e)                │
│  [ ] Linter passes with no warnings                         │
│  [ ] Build succeeds                                         │
│  [ ] Type checking passes (if applicable)                   │
│  [ ] Security review completed (if needed)                  │
│  [ ] Performance acceptable (no regressions)                │
└─────────────────────────────────────────────────────────────┘
```

## Constraints

- **Atomic commits per phase**: Each phase MUST end with a commit
- **No skipping phases**: Follow the sequence even for small features
- **Verification gates**: Do not proceed until verification passes
- **Track everything**: All phases defined upfront in task tracking
- **Clean completion**: No uncommitted changes, all tasks resolved

## Implementation Output

At completion, document the implementation:

```markdown
## Implementation Summary

### What Was Built
[Brief description of the feature]

### Files Changed
- `path/to/file` — [what changed]

### Architecture Decisions
[Key decisions made and rationale]

### Testing
- [Tests added]
- [Coverage notes]

### Verification
- [ ] All diagnostics clean
- [ ] Tests passing
- [ ] Build succeeds

### Commits Made
- [Commit hash] — [Phase X: description]

### Known Limitations
[Any constraints or future work]

### Next Steps
[Follow-up tasks if any]
```

See `references/templates/implementation-summary.md` for the full template.

## Example

```
Feature: Add user notification preferences

Phase 1: Requirements Analysis
  ✓ Explored existing notification code
  ✓ Found user preferences patterns
  ✓ Clarified: email, push, in-app toggles needed
  ✓ Committed: docs/specs/notification-prefs.md

Phase 2: Architecture Decision
  ✓ Evaluated: column per pref vs JSON blob vs separate table
  ✓ Decision: separate preferences table (flexibility)
  ✓ Committed: docs/adr/007-notification-preferences.md

Phase 3: Implementation
  Backend:
    ✓ Created preferences table migration
    ✓ Added PreferencesService
    ✓ Added API endpoints
  Frontend:
    ✓ Added preferences UI component
    ✓ Integrated with settings page
  ✓ Committed: feat: add notification preferences

Phase 4: Integration
  ✓ Connected UI to API
  ✓ Added e2e tests
  ✓ Committed: chore: integrate notification preferences

Phase 5: Verification
  ✓ All tests pass (47/47)
  ✓ Lint clean
  ✓ Build succeeds
  ✓ Committed: fix: address lint warnings

Phase 6: Documentation & Cleanup
  ✓ Updated API docs
  ✓ Created implementation summary
  ✓ All tasks completed
  ✓ Committed: docs: notification preferences

Implementation complete!
```

Begin by gathering context about the feature to implement and setting up task tracking with all phases defined.
