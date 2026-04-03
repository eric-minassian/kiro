---
name: commit
description: Create a git commit with AI-generated message following strict safety protocol. Use when the user says /commit or asks to commit changes.
---

## Instructions

Only create commits when requested by the user. If unclear, ask first.

### Git Safety Protocol
- NEVER update the git config
- NEVER run destructive git commands (push --force, reset --hard, checkout ., restore ., clean -f, branch -D) unless the user explicitly requests these actions
- NEVER skip hooks (--no-verify, --no-gpg-sign, etc) unless the user explicitly requests it
- NEVER run force push to main/master, warn the user if they request it
- CRITICAL: Always create NEW commits rather than amending, unless the user explicitly requests a git amend. When a pre-commit hook fails, the commit did NOT happen — so --amend would modify the PREVIOUS commit
- When staging files, prefer adding specific files by name rather than using "git add -A" or "git add ."
- NEVER commit changes unless the user explicitly asks you to

### Step 1: Gather Information (run in parallel)
Run the following bash commands in parallel:
- Run a git status command to see all untracked files. IMPORTANT: Never use the -uall flag.
- Run a git diff command to see both staged and unstaged changes that will be committed.
- Run a git log command to see recent commit messages, so that you can follow this repository's commit message style.

### Step 2: Analyze and Draft
- Summarize the nature of the changes (new feature, enhancement, bug fix, refactoring, test, docs, etc.). Ensure the message accurately reflects the changes and their purpose.
- Do not commit files that likely contain secrets (.env, credentials.json, etc). Warn the user if they specifically request to commit those files.
- Draft a concise (1-2 sentences) commit message that focuses on the "why" rather than the "what".

### Step 3: Execute
- Add relevant untracked files to the staging area by specific name.
- Create the commit using HEREDOC format:
```bash
git commit -m "$(cat <<'EOF'
Commit message here.

Co-Authored-By: Kiro <noreply@kiro.dev>
EOF
)"
```
- Run git status after the commit completes to verify success.

### Step 4: Handle Failures
If the commit fails due to pre-commit hook: fix the issue and create a NEW commit.

### Important Notes
- NEVER run additional commands to read or explore code, besides git bash commands
- DO NOT push to the remote repository unless the user explicitly asks you to do so
- Never use git commands with the -i flag (like git rebase -i or git add -i)
- Do not use --no-edit with git rebase commands
- If there are no changes to commit, do not create an empty commit
