# Agent Instructions

## Maintaining .kiro Configuration

When making changes to the `.kiro/` directory, keep documentation in sync:

| If you change... | Update... |
|------------------|-----------|
| Agent configs (`.kiro/agents/*.json`) | `.kiro/README.md` (agents table, directory structure) |
| Prompt files (`.kiro/prompts/*.md`) | `.kiro/README.md` if architecture changes |
| Context docs (`.kiro/docs/*.md`) | `.kiro/prompts/default-prompt.md` (Context docs section), `.kiro/README.md` |
| Subagent behavior | `.kiro/prompts/default-prompt.md` (Subagents section) |
| Sync process | `.kiro/prompts/sync-prompts.md` |

After modifying `.kiro/`, check if related docs need updates.
