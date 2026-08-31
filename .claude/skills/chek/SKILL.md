---
name: chek
description: Run the CHEK multi-agent code audit — split the codebase into domains, run a read-only checker fleet in parallel, gap-find, fix in a separate role, judge with an at-least-five-seat council (Bad_Apple's security veto is a hard gate), and pin every closed problem with a regression test proven RED-before / GREEN-after. Use when the user asks for a thorough / rigorous / exhaustive code audit, a "CHEK" or "ЧЕК", "audit this codebase", "find every bug", or wants a review far deeper than a single-pass read. Not for a quick look or a single-file glance.
---

# CHEK

CHEK is a full protocol, not a prompt. Do not improvise a shorter version.

## How to run it

1. Read `CHEK_PROTOCOL.md` (repo root) in full. It defines every step, every gate, and every agent prompt.
2. Execute its steps verbatim, in order. The hard rules:
   - Fix nothing before the fixer step. Commit nothing before the final step — the human triggers the commit.
   - Never collapse the fleet into one agent. Never run the Step 10 council with fewer than five seats.
   - The Step 12 stash check (regression test RED on pre-fix code, GREEN after) is the only LLM-judgment-free
     gate and is the default definition of "resolved".
   - Bad_Apple's `SCORE: X/10` below the veto threshold (8) is a hard veto — no appeal by agents.
3. The agent role bodies are in `agents/*.md` — paste each into its subagent call (or use `CHEK_PROTOCOL.md`'s
   inline prompt for a step that has one). The mandatory council seats are `agents/{melchior,balthasar,casper,
   apple,bad-apple}.md`; a project may add more seats, never fewer.
4. Problem registry: `chek_open.md` / `chek_never.md` / `chek_later.md` at the project root (create empty ones if
   missing — Step 1 / Step 13 own their format). A problem lives in exactly one.

## Scope from the invocation

- no scope -> whole project.
- "all" / "everything" -> also re-check the suppressed (never/later) registries.
- a file / module / topic -> that is the scope.
- a user-supplied bug list -> skip the find/plan phase, start at the fixer step with that list as the report.

## Mechanism

`CHEK_PROTOCOL.md`'s model/tier names are the Claude Code concrete form (delegated tier = Sonnet-class, top tier
= Opus-class). On a different agentic CLI, map the roles to its own multi-agent primitive — the roles, order and
gates do not change. The smallest model tier is never used for a CHEK role.
