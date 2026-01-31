---
description: Perform a comprehensive code review of PRs, commits, local changes, or the codebase
argument-hint: "[PR number] | [Commit SHA]"
---

# Code Review: {{argument}}

## CRITICAL INSTRUCTION
**ACTIVATE SKILL** `code-review` before proceeding with the review logic. Use the instructions and dimensions defined in that skill.

---

## Phase 1: Context Detection

1. **Determine the review target:**
   - **If `{{argument}}` is a number** (e.g., 42): Review Pull Request #{{argument}}.
   - **If `{{argument}}` is a SHA** (e.g., a1b2c3d): Review commit {{argument}}.
   - **If `{{argument}}` is empty:**
     - Check `git status --porcelain`.
     - **If changes exist:** Review uncommitted changes (dirty status).
     - **If no changes exist:** Review the entire codebase (high-level audit).

2. **Gather the Diff/Source:**
   - **PR:** Use `gh pr diff {{argument}}` or `pull_request_read(method="get_diff")`.
   - **Commit:** Use `git show {{argument}}`.
   - **Uncommitted:** Use `git diff HEAD`.
   - **Entire Codebase:** Perform a structured walkthrough of the project root, identifying core services, APIs, and entry points.

---

## Phase 2: Review Execution

Follow the `code-review` skill's multi-dimensional analysis:

1. **Correctness:** Logic, edge cases, error handling.
2. **Security:** Injections, secrets, auth, data privacy.
3. **Performance:** Efficiency, memory, database usage.
4. **Reliability:** Graceful failure, timeouts, resource cleanup.
5. **Maintainability:** Readability, patterns, naming, DRY.
6. **Testing:** Coverage, quality, edge case testing.

---

## Phase 3: Reporting

Synthesize the findings into a structured report using the template in `skills/code-review/references/code-review-template.md`.

- **Verdict:** APPROVE, NEEDS WORK, or BLOCK.
- **Categorization:** Group findings by severity (Critical to Low).
- **Actionable:** Provide specific code fixes for identified issues.
- **Positive Reinforcement:** Acknowledge strengths and good patterns.

---

## Begin Review

Target: {{argument}}

Start by detecting the context and gathering the diff or source code.
