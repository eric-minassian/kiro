---
name: create-pr
description: Create a GitHub pull request analyzing all commits on the branch. Use when the user says /create-pr or asks to create a PR.
---

## Instructions

Use the gh command via shell for ALL GitHub-related tasks.

### Step 1: Gather Information (run in parallel)
Run the following bash commands in parallel:
- Run a git status command to see all untracked files (never use -uall flag)
- Run a git diff command to see both staged and unstaged changes that will be committed
- Check if the current branch tracks a remote branch and is up to date with the remote, so you know if you need to push to the remote
- Run a git log command and `git diff [base-branch]...HEAD` to understand the full commit history for the current branch (from the time it diverged from the base branch)

### Step 2: Analyze and Draft
Analyze all changes that will be included in the pull request, making sure to look at all relevant commits (NOT just the latest commit, but ALL commits that will be included in the pull request!!!), and draft a pull request title and summary:
- Keep the PR title short (under 70 characters)
- Use the description/body for details, not the title

### Step 3: Create PR (run in parallel)
- Create new branch if needed
- Push to remote with -u flag if needed
- Create PR using gh pr create with the format below. Use a HEREDOC to pass the body to ensure correct formatting.

```bash
gh pr create --title "the pr title" --body "$(cat <<'EOF'
## Summary
<1-3 bullet points>

## Test plan
[Bulleted markdown checklist of TODOs for testing the pull request...]

Generated with Kiro CLI
EOF
)"
```

### Step 4: Return Result
Return the PR URL when you're done, so the user can see it.
