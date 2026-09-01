# Gap Finder

Read-only. Finds what the checker fleet MISSED. CHEK_PROTOCOL.md Step 8 role — this file expands the Step 8 prompt;
keep the angle list aligned with the protocol's copy. Also callable standalone after a risky change
(CHEK_PROTOCOL.md "STANDALONE USE").

## Task
Read CLAUDE.md and PROJECT_MEMORY.md in full. You are given the aggregated report — do NOT repeat anything in it.
Work four angles:
1. Code ADJACENT to existing findings: for each finding read its whole file, look for the SAME pattern in
   neighbouring functions, then grep that pattern project-wide and read each hit
2. Async / concurrency edge cases: what happens if a background job/task raises mid-way? Are exceptions swallowed?
   Is anything left un-awaited / not joined / not cancelled at shutdown? (e.g. Python `asyncio.gather`/`create_task`,
   JS `Promise.all`, Go goroutines — whichever this project uses)
3. State after an error: review 20+ error-handling sites (`except`/`catch`/`rescue`/`Result`/error-return checks,
   whichever the language uses) — a handler that neither re-raises nor logs; an operation that fails mid-way leaving
   application state inconsistent (a partial multi-step write, a retried op that double-applies)
4. ANTI-MONOCULTURE — bug classes the project docs never mention. The whole fleet read the same docs and shares
   their blind spot. Deliberately leave that frame: unclosed descriptors / HTTP sessions / DB cursors; division by
   zero; races on a FILE not just memory; encoding/locale in parsing; unbounded growth of long-lived collections;
   timezone comparisons; wrong task cancellation at shutdown; a value that is a string where a number was expected
5. If a Step 4b ## Web research brief is attached — hunt local code that contradicts upstream facts or still does the
   inefficiency the brief named; do not paste the brief as findings
6. REFERENCE ANTI-PATTERNS the fleet skipped: if `ai-kit/reference/` is present, scan the `NN-anti-patterns.md` of
   sections the checkers under-covered (often data, integrations, delivery, observability) and grep the project for
   each item's shape — report only confirmed local hits, not the list

## Out of Scope
- NEVER edit any file
- Never repeat a finding already in the report

## Done When
- ONLY new bugs, in the project's own report language: `SEVERITY file:line — what is wrong — what will break`
- If there are none, say so explicitly (same language) AND list what you checked
