---
name: review-pr
description: Review a pull request thoroughly for correctness, security, performance, and style. Use when the user says /review-pr or asks to review a PR. Takes a PR number or URL as argument.
---

## Instructions

Use the gh command via shell for ALL GitHub-related tasks. If given a Github URL use the gh command to get the information needed.

### Step 1: Gather PR Information (run in parallel)
- Use `gh pr view <number> --json title,body,baseRefName,headRefName,files,additions,deletions,commits` to get PR metadata
- Use `gh pr diff <number>` to get the full diff
- Use `gh api repos/{owner}/{repo}/pulls/{number}/comments` to read existing review comments

### Step 2: Read Context
For each significantly changed file in the diff, read the full file to understand the surrounding context. Don't review changes in isolation — understand:
- The module's purpose and architecture
- Existing patterns and conventions in the file
- How the changed code interacts with the rest of the codebase

### Step 3: Analyze the Changes

Check for:

**Correctness**
- Does the code do what the PR description claims?
- Are there logic errors, off-by-one errors, or missed edge cases?
- Are error paths handled correctly?
- Is state managed correctly (no stale reads, no race conditions)?

**Security**
- Any vulnerabilities introduced? (injection, XSS, secrets in code, insecure deserialization)
- Are inputs validated at system boundaries?
- Are authentication/authorization checks present where needed?

**Performance**
- Any N+1 queries, unnecessary re-renders, or hot-path bloat?
- Are there missed concurrency opportunities?
- Any unbounded data structures or missing pagination?

**Style & Patterns**
- Does it follow the project's existing conventions?
- Are names clear and consistent with surrounding code?
- Is there unnecessary complexity, premature abstraction, or dead code?

**Tests**
- Are changes tested? Are the tests meaningful (not just asserting mocks)?
- Are edge cases covered?
- Do tests test behavior, not implementation details?

### Step 4: Provide Review

Format your review as:

**Summary**: One-line description of what the PR does

**Assessment**: Overall quality — approve, request changes, or comment

**Issues Found** (if any):
For each issue, include:
- File and line reference (file_path:line_number)
- Category: bug, security, performance, style, suggestion, nit
- Specific description of the problem
- Suggested fix (if you have one)

Prioritize issues by severity. Lead with bugs and security issues. Put nits last.

**Positive Notes** (if any):
Call out well-written code, good architectural decisions, or thorough test coverage. Be specific.

Keep the review constructive, specific, and actionable. Don't nitpick formatting that a linter should catch.
