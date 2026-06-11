# Guide

How to create agents and skills for Claude Code.

## Agents

Agents are Claude subagents with a persistent persona, a restricted tool set, and opinionated behavioral rules. They live in `agents/<name>.md` and are available as named subagents in Claude Code.

### Frontmatter

```yaml
---
name: Reviewer
description: One-line summary — Claude uses this to decide when to invoke the agent automatically
color: green        # red | orange | yellow | green | blue | purple
model: sonnet       # sonnet | haiku | opus
tools: Read, Bash, Glob, Grep, WebSearch
---
```

`description` is the most important field. Claude reads it to match the agent against tasks — write it as a capability statement, not a job title.

Available tools: `Read`, `Edit`, `Write`, `Bash`, `Glob`, `Grep`, `WebSearch`, `Agent`

### Body

The body is the agent's system prompt. Write it like you're briefing a colleague: what they do, what they don't do, and what format their output should take.

A useful structure:

```markdown
<One-line role statement.>

## On invocation
Steps the agent takes when first called.

## Output format
The exact shape of the agent's response.

## Responsibilities
What the agent does and does not do.

## Stop / escalate when
Conditions that should cause the agent to pause and ask rather than proceed.
```

Be explicit about handoffs if the agent is part of a pipeline. Vague instructions produce vague behavior.

---

## Skills

Skills are slash commands invoked as `/skill-name` inside Claude Code. They live in `skills/<name>/SKILL.md`.

### Frontmatter

```yaml
---
name: pr-description
description: One-line summary of what this skill does
disable-model-invocation: true   # optional — see below
---
```

Set `disable-model-invocation: true` when the skill should run without spawning a new model context — useful for lightweight tasks that only need shell output and a single model call.

### Shell context blocks

Skills can inject live shell output using `` !`command` `` in the body. These run at invocation time and make the output available to Claude before it executes the task:

```markdown
## Context

- Branch: !`git branch --show-current`
- Diff: !`git diff main...HEAD 2>/dev/null`
- Open PRs: !`gh pr list --author "@me" --state open --json title,url 2>/dev/null`
```

Use this to give the skill situational awareness without making the user copy-paste context manually.

### Body

The body defines the task. Be specific about format and rules:

```markdown
## Context
<shell context blocks>

## Task
<what Claude should produce>

**Format:**
<exact output shape, ideally with an example>

**Rules:**
- <constraint>
- <constraint>
```

---

## Tips

**Agents vs skills** — use an agent when the behavior needs a persistent persona, multi-step reasoning, or its own tool restrictions. Use a skill when you just need a repeatable task with a defined output.

**Scope agents tightly** — the narrower the responsibility, the more reliably the agent performs it. An agent that does one thing well is more useful than one that does five things adequately.

**Write stop conditions** — every agent should know when to pause and ask rather than proceed. Missing stop conditions are the main cause of agents doing the wrong thing confidently.

**Name skills after the action** — `/pr-description`, `/commit`, `/standup` — not `/helper` or `/tools`.
