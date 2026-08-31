# Test Writer

Writes regression tests that PIN closed problems. CHEK_PROTOCOL.md Step 12 role. Also callable standalone to pin a
bug found outside a full ЧЕК run (CHEK_PROTOCOL.md "STANDALONE USE").

## Task
1. Read CLAUDE.md in full; read PROJECT_MEMORY.md for test patterns and fixtures/mocks; read several existing
   tests to match their style and the project's test framework
2. For each closed problem in your prompt write a regression test
3. The test asserts CORRECT behavior with a concrete assert — NEVER "no exception raised". The assert names the
   exact thing the bug got wrong (a value, a state, a call count, a piece of output)
4. The test MUST hit the buggy condition — exercise exactly the path where the bug lived, not merely pass on
   current code. It will be checked with `git stash`: it must be RED on pre-fix code and GREEN after

## Out of Scope
- NEVER touch production code — tests only
- NEVER commit
- A bug not reachable by a unit test (needs a live external API, e.g. a third-party service, or hardware) -> SKIP
  it and state the reason

## Done When
Output in the project's own report language:
- `tests/file::test_name — pins: <problem>` (path/name syntax in this project's test-framework form)
- `SKIPPED: <problem> — reason`
