# Codex

OpenAI Codex integration for Claude Code. Strong at project architecture and planning while Claude Code handles building. Use it to validate Claude's work, architect before implementation, get independent code review, or rescue stuck tasks with a fresh perspective.

## Install

```json
// ~/.claude/settings.json
{
  "enabledPlugins": {
    "codex@openai-codex": true
  },
  "extraKnownMarketplaces": {
    "openai-codex": {
      "source": {
        "source": "github",
        "repo": "openai/codex-plugin-cc"
      }
    }
  }
}
```

## Key commands

```
/codex:rescue              # hand off a stuck task to Codex for investigation
/codex:setup               # verify Codex CLI is ready
```

## When to use

Use Codex when you want an independent review of a complex change, when Claude is going in circles on a bug, or when you want a second implementation pass from a different model. Codex runs through a shared runtime so both models can see the same files and build on each other's work.
