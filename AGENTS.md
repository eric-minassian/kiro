# Agent Instructions

## Kiro CLI Documentation

When working with Kiro CLI configuration, agents, or features, reference the official documentation:

- **Sitemap**: https://kiro.dev/sitemap.xml
- **Custom Agents**: https://kiro.dev/docs/cli/custom-agents/
- **Configuration Reference**: https://kiro.dev/docs/cli/custom-agents/configuration-reference/
- **Steering Files**: https://kiro.dev/docs/cli/steering/
- **Context Management**: https://kiro.dev/docs/cli/chat/context/
- **Subagents**: https://kiro.dev/docs/cli/chat/subagents/

Fetch these docs when you need context about Kiro features or configuration options.

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
