# verify-before-done

> "Done" has to be earned with execution evidence, not asserted.

An agent skill that stops the most common failure in AI coding: writing code, explaining why it *should* work, and handing it back without ever running it.

## What it does

Makes a task incomplete until the agent has shown real command output proving the change works, compared against the behaviour before.

| Stage | What the agent must do |
|-------|------------------------|
| 1. Baseline | Reproduce the bug / capture current behaviour first |
| 2. Implement | Keep the diff scoped |
| 3. Execute | Run the exact code path that changed, not a neighbour |
| 4. Compare | State before → after in one line |
| 5. Regression | Run the cheap broader checks (tests, lint, types) |
| 6. Report | Commands run, real output, and anything left **unverified** |

It also lists hard-stop conditions (no command run since last edit, "should work" phrasing, a failing test ignored, shared code tested at one call site only) and a fast path so typo fixes don't need a full test run.

## When it triggers

Any code change, bug fix, feature, refactor, script, API integration, ERP/Odoo module, or infrastructure config edit, whether or not you asked for tests.

## Example

Without the skill: *"I've fixed the null reference; it should now work."*

With it:

```
Before: AttributeError: 'NoneType' object has no attribute 'partner_id'
Ran:    pytest tests/test_order.py -q
After:  3 passed. Order confirms, partner_id populated.
Not verified: payment gateway (no sandbox credentials here).
```

## Files

- `SKILL.md`: the whole skill, no extra dependencies.

## Install

```bash
git clone https://github.com/anaslari23/verify-before-done.git
```

Copy the `verify-before-done` folder into your tool's skills directory, then restart it or open a new session.

| Tool | Global (all projects) | Project only |
|------|----------------------|--------------|
| Claude Code | `~/.claude/skills/verify-before-done/` | `.claude/skills/verify-before-done/` |
| Codex CLI | `~/.codex/skills/verify-before-done/` | `.codex/skills/verify-before-done/` |
| Cursor | `~/.cursor/skills/verify-before-done/` | `.cursor/skills/verify-before-done/` |
| Antigravity | `~/.gemini/antigravity/skills/verify-before-done/` | `.agent/skills/verify-before-done/` |

One-liner (Claude Code, global):

```bash
git clone https://github.com/anaslari23/verify-before-done.git ~/.claude/skills/verify-before-done
```

- **Cursor without Agent Skills:** paste `SKILL.md` into `.cursor/rules/verify-before-done.mdc`.
- **Claude.ai / Desktop:** zip the `verify-before-done` folder and upload it under Settings → Capabilities → Skills.
- Paths vary by tool version; check your tool's skills docs if one doesn't load.
- To update later: `git -C <install path> pull`.
