# Tessera

A personal collection of Claude Code agent and skill definitions.

Fork it and add your own. See [GUIDE.md](GUIDE.md) for how to create agents and skills.

## Setup

Symlink everything into your Claude Code config so your definitions stay in sync with this repo:

```bash
REPO="$HOME/code/claude-tessera" && \
  mkdir -p ~/.claude/agents ~/.claude/skills && \
  ln -sf "$REPO/agents/"*.md ~/.claude/agents/ && \
  ln -sf "$REPO/skills/"* ~/.claude/skills/
```

Agents are available immediately. Skills are available as `/skill-name` slash commands.

## Structure

```
claude-tessera/
├── agents/   # Agent definitions (.md)
└── skills/   # Skill definitions (each in its own folder)
```

## Requirements

- [Claude Code](https://claude.ai/code) installed and configured
- A valid Anthropic API key or Claude Pro/Max subscription
