# Sync Prompts from Claude Code

Update the Kiro CLI prompts to match the latest Claude Code system prompts.

## Source Repository

https://github.com/Piebald-AI/claude-code-system-prompts

## Files to Sync

| Claude Code Source                                   | Kiro Target                       |
| ---------------------------------------------------- | --------------------------------- |
| `system-prompts/agent-prompt-explore.md`             | `.kiro/prompts/explore-prompt.md` |
| `system-prompts/agent-prompt-plan-mode-enhanced.md`  | `.kiro/prompts/plan-prompt.md`    |
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

3. **IMPORTANT: Preserve Kiro-specific sections in default-prompt.md**

   The default prompt contains custom sections that are NOT from Claude Code. When updating, you MUST preserve these sections:

   ```
   # Subagents
   [PRESERVE THIS ENTIRE SECTION]

   # Context docs
   [PRESERVE THIS ENTIRE SECTION]
   ```

   Insert the translated Claude Code content, then append these Kiro-specific sections before "# Tool usage policy".

4. Write the translated prompts to the corresponding Kiro target files.

5. Show a diff summary of what changed.

6. Do NOT commit - let the user review first.

## Kiro-Specific Files (Do Not Sync)

These files are Kiro-only and should NOT be overwritten by sync:

| File                       | Purpose                                              |
| -------------------------- | ---------------------------------------------------- |
| `.kiro/docs/playwright.md` | Playwright testing best practices (loaded on-demand) |
| `.kiro/agents/*.json`      | Agent configurations with Kiro-specific settings     |

## Tool Name Mapping Reference

| Claude Code | Kiro CLI        |
| ----------- | --------------- |
| Read        | read            |
| Write       | write           |
| Edit        | edit            |
| Bash        | shell           |
| Glob        | glob            |
| Grep        | grep            |
| Task        | (subagents)     |
| TodoWrite   | (task tracking) |
