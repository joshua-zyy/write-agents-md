# Tool Conventions: Agent Instruction Files

How each agent platform discovers instruction files. Read this when adapting the output to the user's specific tools.

## Comparison

| Tool | Files | Notes |
| --- | --- | --- |
| Claude Code | `CLAUDE.md`, `.claude/CLAUDE.md` | Does **not** read `AGENTS.md` directly; can import it with `@AGENTS.md` |
| OpenAI Codex | `AGENTS.md`, `AGENTS.override.md` | Directory-hierarchy merge; closer files take precedence |
| Cursor | `.cursor/rules/*.mdc` (recommended) | `.cursorrules` is legacy; root `AGENTS.md` is also supported |
| Gemini CLI | `GEMINI.md` | Hierarchical loading, `@` imports, custom file names |
| Aider | none automatic | `--read AGENTS.md` or `CONVENTIONS.md` |
| Pi | `AGENTS.md` (global + project), `CLAUDE.md` | Native support, layered loading |
| VS Code / Copilot | root + experimental nested `AGENTS.md` | `/init` command generates instructions |

## The Open Standard

- [agentsmd/agents.md](https://github.com/agentsmd/agents.md) — the mainstream open convention: pure Markdown, no required fields, root plus nested files.
- The AGENTS.md v1.1 proposal (issue #135) adds explicit hierarchy, inheritance, precedence, and progressive disclosure — the spec is still settling, so target today's widely-supported baseline.

## Adaptation Patterns

Use **AGENTS.md as the single source of truth** and adapt per tool — prefer symlinks over copies: one source, no drift.

- **Claude Code**: `ln -s AGENTS.md CLAUDE.md`, or `@AGENTS.md` imports in `.claude/CLAUDE.md`
- **Gemini CLI**: `ln -s AGENTS.md GEMINI.md`, or configure a custom file name
- **Cursor**: keep AGENTS.md for portability; optionally mirror key rules into `.cursor/rules/*.mdc` if the user relies on Cursor's rule UI
- **Aider**: launch with `--read AGENTS.md`
- **Codex**: no action needed — AGENTS.md is native
- **Pi**: no action needed — AGENTS.md is native
- **VS Code**: no action needed — root AGENTS.md is supported

Note: symlinked instruction files work in the repository itself; the packaging restriction on symlinks only applies to distributing agent skills, not to repo files.
