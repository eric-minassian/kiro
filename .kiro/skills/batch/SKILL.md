---
name: batch
description: Research and plan a large-scale change, then execute it in parallel across multiple isolated agents that each open a PR. Use when the user wants to make a sweeping, mechanical change across many files (migrations, refactors, bulk renames) that can be decomposed into independent parallel units.
---

## User Instruction

$instruction

## Phase 1: Research and Plan (Plan Mode)

Enter plan mode, then:

1. **Understand the scope.** Launch one or more subagents (in the foreground — you need their results) to deeply research what this instruction touches. Find all the files, patterns, and call sites that need to change. Understand the existing conventions so the migration is consistent.

2. **Decompose into independent units.** Break the work into 5–30 self-contained units. Each unit must:
   - Be independently implementable (no shared state with sibling units)
   - Be mergeable on its own without depending on another unit's PR landing first
   - Be roughly uniform in size (split large units, merge trivial ones)

   Scale the count to the actual work: few files → closer to 5; hundreds of files → closer to 30. Prefer per-directory or per-module slicing over arbitrary file lists.

3. **Determine the e2e test recipe.** Figure out how a worker can verify its change actually works end-to-end — not just that unit tests pass. Look for:
   - A dev-server + curl pattern (for API changes: start the server, hit the affected endpoints)
   - An existing e2e/integration test suite the worker can run
   - CLI verification (for CLI changes: launch the app, exercise the changed behavior)

   If you cannot find a concrete e2e path, ask the user how to verify this change end-to-end. Offer 2–3 specific options based on what you found. Do not skip this — the workers cannot ask the user themselves.

   Write the recipe as a short, concrete set of steps that a worker can execute autonomously.

4. **Write the plan.** Include:
   - A summary of what you found during research
   - A numbered list of work units — for each: a short title, the list of files/directories it covers, and a one-line description of the change
   - The e2e test recipe (or "skip e2e because …" if the user chose that)
   - The exact worker instructions you will give each agent

5. Present the plan for user approval.

## Phase 2: Spawn Workers (After Plan Approval)

Once the plan is approved, spawn one background agent per work unit. Launch them all in a single message block so they run in parallel.

For each agent, the prompt must be fully self-contained. Include:
- The overall goal (the user's instruction)
- This unit's specific task (title, file list, change description — copied verbatim from your plan)
- Any codebase conventions you discovered that the worker needs to follow
- The e2e test recipe from your plan (or "skip e2e because …")
- The worker instructions below, copied verbatim:

```
After you finish implementing the change:
1. **Simplify** — Review and clean up your changes.
2. **Run unit tests** — Run the project's test suite. If tests fail, fix them.
3. **Test end-to-end** — Follow the e2e test recipe from the coordinator's prompt. If the recipe says to skip e2e for this unit, skip it.
4. **Commit and push** — Commit all changes with a clear message, push the branch, and create a PR with `gh pr create`. Use a descriptive title. If `gh` is not available or the push fails, note it in your final message.
5. **Report** — End with a single line: `PR: <url>` so the coordinator can track it. If no PR was created, end with `PR: none — <reason>`.
```

## Phase 3: Track Progress

After launching all workers, render an initial status table:

| # | Unit | Status | PR |
|---|------|--------|----|
| 1 | <title> | running | — |
| 2 | <title> | running | — |

As completion notifications arrive, update the table with status (done / failed) and PR links.

When all agents have reported, render the final table and a one-line summary (e.g., "22/24 units landed as PRs").
