---
name: verify-before-done
description: Enforces that any coding, debugging, or configuration task is actually run and verified before it can be reported as complete. Use this for EVERY code change, bug fix, feature implementation, refactor, or config edit — not just when the user asks for testing. Trigger it silently on any task where you are about to write, edit, or generate code, scripts, Odoo/ERP modules, API integrations, or infrastructure config. If you catch yourself about to say "this should work," "I've implemented X," or hand the task back without having actually executed something, that is the signal this skill exists to block. Applies whether or not the user explicitly asked for tests.
---

# Verify Before Done

## The problem this exists to stop

Agents write code, reason convincingly about why it should work, and then
report the task as finished — without ever running it. Confidence in prose
is not evidence. This skill makes "done" a status you have to *earn* with
a test run, not one you can *assert*.

**Hard rule: a task is not complete until you have produced execution
evidence — real command output, not predicted output — showing the
change works, and you have compared it against the prior behavior.**

If you have not run anything, you are not done. It does not matter how
sure you are.

## The workflow (do not skip stages)

### 1. Baseline — capture "before"
Before touching code, know what "currently broken" or "currently absent"
looks like. Run the failing test, reproduce the bug, or run the existing
behavior once, and note the actual output. If no test/repro exists yet,
write the smallest possible one (a script, curl call, unit test, Odoo
shell command, console log) — this takes two minutes and is non-optional.
You cannot compare "before vs after" later if you never captured "before."

### 2. Implement
Make the change. This is the part every agent already does well — no
special guidance needed here beyond: keep the diff scoped to what's needed.

### 3. Execute — capture "after"
Run the actual code path you changed. Not a related test, not "the build
passed" — the specific behavior you were asked to fix or add.
- Unit/integration tests exist → run them, paste the real pass/fail output.
- No tests exist → write a minimal verification script or manual repro
  (curl request, CLI invocation, REPL session, Odoo shell/`-i`/`-u` module
  reload) and run it. Delete throwaway scripts after, or keep them if
  useful — your call, but they must actually execute.
- UI change → describe the concrete interaction you exercised and the
  observed result, not the expected one.
- If execution is genuinely impossible in this environment (no DB, no
  credentials, sandboxed), say so explicitly and flag it as **unverified**
  in your report — never silently skip to "done."

### 4. Compare — before vs after, explicitly
State, in one or two lines, what changed in observed behavior:
- "Before: `AttributeError: 'NoneType' object has no attribute 'partner_id'`.
  After: order confirms, `partner_id` populated, test passes (3/3)."
Not "the fix should resolve the null reference." The comparison is the
proof of causation — it shows the change is what fixed it, not that
something unrelated happened to pass.

### 5. Regression check
Before reporting done, run whatever broader check is cheap and available:
existing test suite, adjacent module tests, a lint/type-check pass. You're
not re-deriving the whole system, just making sure the fix didn't break a
neighbor. If the codebase has no test suite at all, say so — that's useful
information for the report, not a reason to skip step 3.

### 6. Report — with evidence attached
Every "done" report includes:
- What you changed (brief).
- The actual command(s) you ran to verify it.
- The actual output (trimmed if long, but real).
- Before → after comparison in one line.
- Anything you could NOT verify and why (missing env, no test infra,
  needs manual QA, etc.) — flagged plainly, not buried.

If any of these five items is missing, the task is not finished — go
back and get it, or state clearly that it's blocked and why.

## Hard stop conditions — do not report "done" if:

- You wrote code but ran no command since the last edit.
- Your evidence is "the logic looks correct" / "this should now work" /
  any phrasing that predicts rather than reports.
- A test failed and you moved on without either fixing it or explicitly
  flagging the failure to the user.
- You changed shared/core code (e.g. an Odoo base model, a shared util,
  an API contract) and only tested the one call site you edited.
- You're about to end the turn and the user could reasonably ask "did you
  actually run this?" and the honest answer is no.

## Fast-path for trivial changes

Not everything needs a full regression pass — a one-line typo fix in a
comment or a docstring doesn't need a test suite run. Use judgment: the
bar scales with blast radius. But "trivial" is a claim you should be able
to defend, not a way to skip step 3 on a real logic change. When in doubt,
run it anyway — it's cheaper than being wrong.

## Anti-patterns to actively avoid

- Ending with "Let me know if this works for you" as a substitute for
  checking it yourself first.
- Reporting file-count or line-count changes as if that were evidence of
  correctness ("updated 4 files" tells the user nothing about behavior).
- Treating "it compiled" / "no syntax errors" as equivalent to "it works."
- Testing the happy path only when the task involved an edge case or bug
  report — reproduce the *actual* reported scenario, not an adjacent one.
