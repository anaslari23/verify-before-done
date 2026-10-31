# verify-before-done

Forces the agent to actually run and verify any code, bug fix, or config change before reporting it complete.

The skill itself is `SKILL.md`. Keep the folder together and named `verify-before-done`.

## Install

Clone once, then copy (or symlink) into the skills directory of your tool.

```bash
git clone https://github.com/anaslari23/verify-before-done.git
```

| Tool | Global (all projects) | Project only |
|------|----------------------|--------------|
| Claude Code | `~/.claude/skills/verify-before-done/` | `.claude/skills/verify-before-done/` |
| Codex CLI | `~/.codex/skills/verify-before-done/` | `.codex/skills/verify-before-done/` |
| Cursor | `~/.cursor/skills/verify-before-done/` | `.cursor/skills/verify-before-done/` |
| Antigravity | `~/.gemini/antigravity/skills/verify-before-done/` | `.agent/skills/verify-before-done/` |

### Claude Code
```bash
mkdir -p ~/.claude/skills && cp -R verify-before-done ~/.claude/skills/
```

### Codex
```bash
mkdir -p ~/.codex/skills && cp -R verify-before-done ~/.codex/skills/
```

### Cursor
```bash
mkdir -p ~/.cursor/skills && cp -R verify-before-done ~/.cursor/skills/
```
On older Cursor versions without Agent Skills, paste `SKILL.md` into a rule at `.cursor/rules/verify-before-done.mdc`.

### Antigravity
```bash
mkdir -p ~/.gemini/antigravity/skills && cp -R verify-before-done ~/.gemini/antigravity/skills/
```
For one workspace only, copy to `<project>/.agent/skills/` instead.

### Claude.ai / Claude Desktop
Zip the `verify-before-done` folder and upload it under Settings → Capabilities → Skills.

Restart the tool (or start a new session) after installing. Skill paths vary between tool versions; if yours differs, check its skills documentation.

## Usage

Skills load automatically when your request matches the `description` in the `SKILL.md` frontmatter. You can also ask for it by name (`verify-before-done`).
