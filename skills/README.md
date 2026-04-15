# Building Slash Commands

A slash command is a `SKILL.md` file placed in `~/.claude/skills/<name>/SKILL.md`. When you type `/<name>` in Claude Code, it loads the skill and follows its instructions. No server, no process, just a markdown file that acts as a prompt template.

## Structure

```markdown
---
name: preflight
description: Validate and optimize a Claude Code project
trigger: /preflight
---

# /preflight

What the command does and why.

## Usage

/preflight              # full run
/preflight check        # validate only

## What You Must Do When Invoked

Step-by-step instructions Claude follows when the skill triggers.
Include exact bash commands, expected outputs, and fallback behavior.
```

The frontmatter tells Claude Code when to activate the skill. The body is instructions Claude follows verbatim.

## Example: /preflight

The `/preflight` skill from [claude_preflight](https://github.com/JKSNS/claude_preflight) validates and optimizes a project in 10 steps. Each step is a self-contained check with bash commands Claude executes directly. It checks Ollama connectivity, installs graphify if missing, starts the sync daemon, registers persistence hooks, and prints a clean summary.

The key design principle is that each step is independent. If one fails, the rest still run. Checks use `2>/dev/null || true` for tools that might not be installed.

## Installing

```bash
mkdir -p ~/.claude/skills/your-command
cp SKILL.md ~/.claude/skills/your-command/SKILL.md
```

Then type `/your-command` in Claude Code.

## Tips

Write the skill like you're briefing someone who has never seen the project. Include exact commands, expected outputs, and what to do when something is missing.

Keep steps independent so failures don't cascade. Use subcommands (parsed from the trigger arguments) to let users run specific parts instead of the full pipeline.

Skills are prompts, not code. Claude interprets and executes them. The more explicit and structured the instructions, the more reliable the results.
