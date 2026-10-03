# Tool Conventions: Agent Instruction Files

How agent platforms commonly discover instruction files. Read this when
adapting output for a specific tool.

**The table below is a starting point, not a verified fact sheet.** Naming
and loading behavior change between versions and setups. Before relying on
a row for a real decision, check it against the user's installed version
(release notes, `--help`, or the tool's own docs).

## Commonly Reported Conventions

| Tool | Files | Notes |
| --- | --- | --- |
| Claude Code | `CLAUDE.md`, `.claude/CLAUDE.md` | Commonly reported not to read `AGENTS.md` directly; `@AGENTS.md` imports are one workaround |
| OpenAI Codex | `AGENTS.md` | Reported directory hierarchy with closer files taking precedence |
| Cursor | `.cursor/rules/*.mdc` | `.cursorrules` is legacy; root `AGENTS.md` also reported as supported |
| Gemini CLI | `GEMINI.md` | Reported hierarchical loading and `@` imports |
| Aider | none automatic | `--read AGENTS.md` or a `CONVENTIONS.md` |
| Pi | `AGENTS.md`, `CLAUDE.md`, `AGENTS.override.md`; agent-directory copies apply across working directories | Context files load additively from the agent directory, the working directory, and its parents |
| VS Code / Copilot | root `AGENTS.md` | `/init` generates instructions; nested support is experimental |

Whether nested files merge with the root also varies by tool and version —
confirm before promising a user that their multi-level layout will merge.

## Pi: Additive Context Files, Replaced System-Prompt Files

Read from Pi's `docs/configuration.md` against an installed version in
2026-10 — documented behavior, not verified by test:

- Instruction files (`AGENTS.md`, `CLAUDE.md`) load from the agent
  directory, the working directory, and its parent directories, and apply
  anywhere below their own directory. Loading does not require project
  trust.
- `AGENTS.override.md` replaces `AGENTS.md` or `CLAUDE.md` in the same
  directory only; it does not suppress files from other directories. This
  is the documented way to make a directory's own rules win.
- `SYSTEM.md` and `APPEND_SYSTEM.md` behave differently: a trusted project
  file takes precedence over the agent-directory file, and files with the
  same name are **not** combined.
- Consequence for placement: permissions, safety boundaries, and anything
  that must hold in every project belong in `AGENTS.md`, which is additive.
  Environment or tooling facts whose loss inside one project would be
  tolerable may go in the agent-directory `APPEND_SYSTEM.md`.

## The Open Convention

- [agentsmd/agents.md](https://github.com/agentsmd/agents.md) — pure
  Markdown, no required fields. Check the repository for the current state
  of nested-file and hierarchy support.

## Adaptation Options

Offer cross-tool adaptation only when the user actually uses other tools
and asks for it. No option is universally right — present the tradeoffs
and let the user choose:

- **Import or reference**: keep one `AGENTS.md` and have other tools load
  it (`@AGENTS.md` in Claude Code, `--read` for Aider). No duplication,
  but it depends on the import feature existing in the user's version.
- **Symlink**: `CLAUDE.md -> AGENTS.md`. One source, no drift — but
  symlinks break on some platforms and checkouts (notably some Windows
  setups). Check before creating one.
- **Copy or mirror**: duplicate content per tool. Robust everywhere, but
  copies drift when only one is edited.
