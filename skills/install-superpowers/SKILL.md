---
name: install-superpowers
description: Use when a user wants to install superpowers from a git URL or GitHub repository, or asks how to set up the superpowers plugin on their platform
---

# Installing Superpowers from a Git URL

## Overview

Superpowers can be installed directly from any git repository URL. The steps differ by platform. Identify the platform first, then follow the matching instructions.

## Identify the Platform

If the platform isn't already known, ask:

> "Which agent platform are you installing superpowers into? (Claude Code, Codex, OpenCode, Gemini CLI, Cursor, or GitHub Copilot CLI)"

## Installation by Platform

### Claude Code

Claude Code installs plugins from a marketplace, not directly from git URLs.

**Recommended — official marketplace:**
```bash
/plugin install superpowers@claude-plugins-official
```

**Alternative — register a custom marketplace first:**
```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

**For a fork or custom URL**, clone manually and restart:
```bash
git clone <git-url> ~/.claude/plugins/superpowers
```
Then restart Claude Code.

### Codex

```bash
git clone <git-url> ~/.codex/superpowers
mkdir -p ~/.agents/skills
ln -s ~/.codex/superpowers/skills ~/.agents/skills/superpowers
```

Restart Codex to discover the new skills.

**Windows (PowerShell):**
```powershell
git clone <git-url> "$env:USERPROFILE\.codex\superpowers"
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\superpowers" "$env:USERPROFILE\.codex\superpowers\skills"
```

### OpenCode

Add to your `opencode.json` (global or project-level):

```json
{
  "plugin": ["superpowers@git+<git-url>"]
}
```

Example with a specific fork:
```json
{
  "plugin": ["superpowers@git+https://github.com/example/superpowers.git"]
}
```

Restart OpenCode — it auto-installs and registers all skills.

### Gemini CLI

```bash
gemini extensions install <git-url>
```

To update later:
```bash
gemini extensions update superpowers
```

### Cursor

Cursor uses its built-in marketplace. Search for "superpowers" in the plugin marketplace or run:
```text
/add-plugin superpowers
```

For a fork, publish the fork to a marketplace first; Cursor does not support direct git URL installs.

### GitHub Copilot CLI

```bash
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

## Verify Installation

After installing, start a new session and ask something that should trigger a skill, such as:

> "help me plan this feature"

The agent should automatically invoke the relevant superpowers skill. If it doesn't, check that the plugin loaded correctly for your platform.

## Updating

| Platform | Command |
|----------|---------|
| Claude Code | `/plugin update superpowers` |
| Codex | `cd ~/.codex/superpowers && git pull` |
| OpenCode | Restart (auto-updates) |
| Gemini CLI | `gemini extensions update superpowers` |
| Cursor | Update via plugin marketplace |

## Common Mistakes

**Wrong URL format for OpenCode:** Use `git+https://...` not bare `https://`.

**Codex symlink missing:** If skills aren't found, verify the symlink exists: `ls -la ~/.agents/skills/superpowers`

**Claude Code git URL:** Claude Code doesn't support direct git URL installs via `/plugin`. Use the manual clone method or the official marketplace.
