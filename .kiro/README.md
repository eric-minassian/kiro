# Kiro CLI Configuration

Custom agent configurations for Kiro CLI, mirroring Claude Code's system prompts with Kiro-specific extensions.

## Directory Structure

```
.kiro/
├── agents/           # Agent configurations (JSON)
│   ├── cdk.json      # AWS CDK with MCP tools (opus-4.5)
│   ├── default.json  # Main coding agent (opus-4.5)
│   ├── explore.json  # Fast codebase search (haiku-4.5)
│   ├── plan.json     # Architecture planning (opus-4.5)
│   └── sync-prompts.json  # Syncs prompts from Claude Code repo
├── docs/             # On-demand context (only loaded when needed)
│   ├── cloudscape.md # Cloudscape design system
│   ├── code-review.md # Code review guide
│   └── playwright.md # Playwright testing
└── prompts/          # System prompts referenced by agents
    ├── cdk-prompt.md
    ├── default-prompt.md
    ├── explore-prompt.md
    ├── plan-prompt.md
    └── sync-prompts.md
```

## Agents

| Agent          | Model     | Purpose                                                |
| -------------- | --------- | ------------------------------------------------------ |
| `default`      | opus-4.5  | General coding tasks, bug fixes, refactoring           |
| `explore`      | haiku-4.5 | Fast, read-only codebase exploration                   |
| `plan`         | opus-4.5  | Read-only architecture and implementation planning     |
| `cdk`          | opus-4.5  | AWS CDK with MCP tools (CDK Nag, Solutions Constructs) |
| `sync-prompts` | haiku-4.5 | Updates prompts from Claude Code repo                  |

### Usage

```bash
# Use default agent
kiro

# Use specific agent
kiro --agent explore
kiro --agent plan
```

## Architecture

### Subagent Spawning

The default agent automatically spawns subagents when appropriate:

- **explore**: For finding files, searching code, understanding project structure
- **plan**: For designing features, architectural decisions, implementation planning

Subagents are spawned by the default agent saying "Use the [agent] agent to [task]".

### On-Demand Context

Files in `.kiro/docs/` are NOT auto-loaded. The default agent reads them only when working on related tasks:

| File             | Loaded When                                       |
| ---------------- | ------------------------------------------------- |
| `cloudscape.md`  | Working with Cloudscape design system components  |
| `code-review.md` | Reviewing code, PRs, or providing code feedback   |
| `playwright.md`  | Writing, debugging, or reviewing Playwright tests |

This keeps context usage efficient - docs are only loaded when actually needed.

### Steering Files (Auto-Loaded)

For always-on context, use `.kiro/steering/` instead:

```
.kiro/steering/
├── product.md    # Product context (auto-loaded)
├── tech.md       # Tech stack (auto-loaded)
└── structure.md  # Project structure (auto-loaded)
```

## Prompt Sources

Prompts are translated from [Claude Code system prompts](https://github.com/Piebald-AI/claude-code-system-prompts):

| Claude Code                           | Kiro                |
| ------------------------------------- | ------------------- |
| `agent-prompt-explore.md`             | `explore-prompt.md` |
| `agent-prompt-plan-mode-enhanced.md`  | `plan-prompt.md`    |
| `system-prompt-main-system-prompt.md` | `default-prompt.md` |

### Syncing Updates

When Claude Code prompts are updated:

```bash
kiro --agent sync-prompts
```

This fetches the latest prompts, translates them for Kiro, and preserves Kiro-specific sections.

## Key Practices

### Tool Name Mapping

| Claude Code | Kiro  |
| ----------- | ----- |
| Bash        | shell |
| Read        | read  |
| Write       | write |
| Edit        | edit  |
| Glob        | glob  |
| Grep        | grep  |

### Kiro-Specific Extensions

The default prompt includes sections not in Claude Code:

1. **Subagents** - Instructions for spawning explore/plan agents
2. **Context docs** - Instructions for loading `.kiro/docs/` files on-demand

These are preserved when syncing from Claude Code.

## Self-Maintaining Docs

The `AGENTS.md` file (project root) instructs both Claude Code and Kiro to keep documentation updated when making changes to `.kiro/`. This file is auto-loaded by both tools.

## Installation

Use GNU Stow to symlink to `~/.kiro`:

```bash
cd /path/to/this/repo
stow -t ~ .
```

Or copy `.kiro/` to any project root for project-specific configuration.
