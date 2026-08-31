---
description: Deep code audit with a problem registry (CHEK)
---

Run a CHEK code audit. The protocol body is `CHEK_PROTOCOL.md`; the agent role prompts are `agents/*.md`.

Action: read `CHEK_PROTOCOL.md` and execute its steps verbatim, in order. Fix nothing before the fixer step. Commit
nothing before the final step (the human triggers the commit). Do not skip a step "for brevity". Do not audit the
project yourself instead of running the fleet.

$ARGUMENTS:
- empty -> audit the whole project.
- "all" / "everything" -> also ignore the suppression registries (chek_never.md + chek_later.md).
- a file / module / topic -> that is the audit scope.
- a list of bugs -> skip the find/plan phase, start at the fixer step, use that list as the report.
