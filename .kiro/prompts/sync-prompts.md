# Sync Prompts from Claude Code

Update the Kiro CLI prompts to match the latest Claude Code system prompts.

## Source Repository

https://github.com/Piebald-AI/claude-code-system-prompts

## Files to Sync

| Claude Code Source | Kiro Target |
|-------------------|-------------|
| `system-prompts/agent-prompt-explore.md` | `.kiro/prompts/explore-prompt.md` |
| `system-prompts/agent-prompt-plan-mode-enhanced.md` | `.kiro/prompts/plan-prompt.md` |
| `system-prompts/system-prompt-main-system-prompt.md` | `.kiro/prompts/default-prompt.md` |

## Instructions

1. Fetch each Claude Code prompt from the raw GitHub URLs:
   - https://raw.githubusercontent.com/Piebald-AI/claude-code-system-prompts/main/system-prompts/agent-prompt-explore.md
   - https://raw.githubusercontent.com/Piebald-AI/claude-code-system-prompts/main/system-prompts/agent-prompt-plan-mode-enhanced.md
   - https://raw.githubusercontent.com/Piebald-AI/claude-code-system-prompts/main/system-prompts/system-prompt-main-system-prompt.md

2. For each prompt, create a faithful translation:
   - Remove the YAML frontmatter (the `<!-- ... -->` block at the top)
   - Replace template variables with Kiro equivalents:
     - `${GLOB_TOOL_NAME}` → `glob`
     - `${GREP_TOOL_NAME}` → `grep`
     - `${READ_TOOL_NAME}` → `read`
     - `${BASH_TOOL_NAME}` → `shell`
     - `${EDIT_TOOL_NAME}` → `edit`
     - `${WRITE_TOOL_NAME}` → `write`
     - `${TODO_TOOL_OBJECT}` → (remove or adapt as needed)
     - `${TASK_TOOL_NAME}` → (remove or adapt as needed)
   - Replace "Claude Code" with "Kiro CLI"
   - Remove any conditional blocks (`${...?...:...}`) and keep the relevant content
   - Preserve the exact structure, wording, and formatting otherwise

3. Write the translated prompts to the corresponding Kiro target files.

4. Show a diff summary of what changed.

5. Do NOT commit - let the user review first.

## Tool Name Mapping Reference

| Claude Code | Kiro CLI |
|-------------|----------|
| Read | read |
| Write | write |
| Edit | edit |
| Bash | shell |
| Glob | glob |
| Grep | grep |
| Task | (subagents) |
| TodoWrite | (task tracking) |
