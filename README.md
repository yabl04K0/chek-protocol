# CHEK — a multi-agent code audit protocol

CHEK is a 13-step protocol for auditing a codebase with a fleet of read-only AI agents, converging on verified
fixes, and pinning every closed problem with a regression test that is proven to fail before the fix and pass
after it. It is designed to be **portable**: nothing in it hardcodes a language, a domain, or a specific project's
invariants — a planning step (Step 5) re-derives the audit fleet for whatever codebase it runs against.

This repo packages the protocol for general use. It is a mirror, published deliberately and manually from a
private structure repo that also maintains a broader AI-collaboration doc stack (rules files, a command-trigger
registry, branching gates) — none of that broader stack is here; this repo is CHEK alone.

## What's in this repo

- `CHEK_PROTOCOL.md` — the protocol itself: roles, models, and the full text of Steps 1-13, including every agent
  prompt template used along the way, ready to paste into an agent tool.
- `agents/web-researcher.md` — the Step 4b role: a dedicated web-research agent so external-truth checking (broken
  upstream links, known CVEs, library API drift, "is this actually the recommended pattern") is a first-class part
  of the audit, not an afterthought.
- `.claude/commands/chek.md` — an example slash-command stub for Claude Code. Adapt the trigger wiring to whatever
  tool or convention your own project uses; the protocol body does not depend on this specific stub.

## Why a protocol instead of "just ask the AI to find bugs"

A single agent skimming a whole codebase in one pass finds the obvious things and misses the rest — it runs out of
attention before it runs out of code. CHEK instead:

1. **Splits the work by domain**, sized so each checker agent can read its whole slice in full, not skim it
   (Step 5 plans the split; Step 6 runs the fleet in parallel).
2. **Separates finding from fixing from verifying** into different roles that never overlap, because an agent that
   both writes and reviews its own code will rationalize its own mistakes.
3. **Decorrelates review with a prompt axis, not just a model axis** — two critics with sharply different,
   non-overlapping focuses (regressions-and-diff vs root-cause-and-contracts) catch different things even when
   they share a model family.
4. **Proves a fix is real, not just LLM-approved** — the one check in the whole protocol that carries no LLM
   judgment at all: a regression test must be RED on the pre-fix code and GREEN after (`git stash` and re-run).
   That is the default gate for calling anything "resolved".
5. **Never lets an agent commit** — a human reviews the diff and the totals, and explicitly says go.

## Adopting this protocol in your project

1. Copy `CHEK_PROTOCOL.md` and `agents/web-researcher.md` into your project.
2. Add three registry files at your project root — `chek_open.md`, `chek_never.md`, `chek_later.md` — each holding
   problems in exactly one of: unresolved (with pass counters), permanently won't-fix, or deferred. Give each an
   empty skeleton; CHEK_PROTOCOL.md Step 1 explains the format each file needs in its own header.
3. Wire a trigger in your project's own convention — a slash command, a chat keyword, whatever your tooling
   supports — that tells the AI to read `CHEK_PROTOCOL.md` and execute Steps 1-13 verbatim.
4. Make sure your project has a `CLAUDE.md` (or equivalent rules file) and a `PROJECT_MEMORY.md` (or equivalent
   structure/invariants doc) — several steps read these to focus the audit on what actually matters for your code.
5. The default mechanism in `CHEK_PROTOCOL.md` targets Claude Code's Agent tool (`subagent_type="general-purpose"`,
   an explicit `model` per role). If you run a different agentic CLI, adapt the role-to-invocation mapping — the
   protocol's roles, order, and gates do not change with the mechanism.

## Language

The protocol document is English (LLM-facing docs read better flat and unambiguous). The example severity words
and report shape use English (`CRITICAL|HIGH|MEDIUM`); swap them for your own project's report language if your
team reads audit output in something else — Step 1 notes the 1:1 mapping explicitly.

## License

MIT — see `LICENSE`. Use this in any project, adapt it freely, no attribution required (though a link back is
appreciated).
