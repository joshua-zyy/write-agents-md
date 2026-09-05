# write-agents-md

[English](README.md) | [中文](README.zh-CN.md)

An agent skill for creating or reviewing **AGENTS.md** and related agent instruction files (CLAUDE.md, GEMINI.md) — at global, project, or nested scope.

This is a **guided-writing skill, not an auto-generator**: it gathers evidence read-only, asks only what the evidence cannot answer, and leaves every permission and deletion decision with the user.

## How It Works

**Step 1 — Establish scope.** From the request and available context: global (user-level preferences), project (repo root), or nested; create or review; target files; and the language the user wants the files in — which may differ from the conversation language. Only unknowns that change the output or touch permissions become questions; there is no full interview by default. Global files are built from the user's own preferences, not repository scans. Project work reads only the relevant README, manifests, CI, and existing instruction files, and stops once the evidence suffices.

**Step 2 — Create or review.**

- *Create*: the file is built only from verifiable project facts and explicit user preferences. Authorization or completion gaps that affect the intended workflow become questions with options — never silent decisions. Omit unsupported template sections without unnecessary investigation.
- *Review*: every existing instruction gets a KEEP / DROP / REWRITE verdict with a one-line reason, assessed on five dimensions — provenance, boundary strength, missing permissions, missing completion conditions, and context economics — followed by a complete replacement draft. Unknown history stays "unknown"; ambiguous boundaries go back to the user.

**Step 3 — Apply and verify.** Respect review-only requests; nothing is written without explicit approval, and approved changes need no repeat approval. Check the proposal before delivery and read back any applied files: no final placeholders or invented commands, local links resolve, language is correct, and intent, scope, and permissions match approved decisions. State unverified claims and distinguish configured commands from tested ones.

## Core Principles

1. **Context is a budget** — loaded instructions consume context; loading varies by tool. Keep rules whose value justifies that cost.
2. **Evidence over invention** — commands and preferences must come from the repo or the user, never from template slots.
3. **Permissions are the user's** — material gaps become questions, not defaults; examine effects and scope without discarding intentional tool restrictions.
4. **Completion is bounded** — distinguish continuing, finishing, and asking for review; report unresolved blockers without endless verification.
5. **Templates are optional skeletons** — take what fits, drop the rest; split files only when the content earns it.

## Installation

Copy the whole directory into your agent's skills folder, for example:

- **Pi**: `~/.pi/agent/skills/write-agents-md/`
- **Claude Code**: `~/.claude/skills/write-agents-md/`

Then trigger it by asking your agent to "write an AGENTS.md" or "review my CLAUDE.md".

## Project Structure

```text
write-agents-md/
├── SKILL.md                        # Skill entry: scope, create/review paths, verification
├── assets/templates/               # Optional output skeletons
│   ├── root-AGENTS.md              #   Minimal project root template
│   └── section.md                  #   Domain-rule section template
└── references/                     # On-demand reference material
    ├── PRINCIPLES.md               #   Why instruction files stay small
    ├── TOOL-CONVENTIONS.md         #   Per-tool loading behavior (unverified starting point)
    └── EXAMPLES.md                 #   Worked examples: global review, create, simplify
```

## License

[MIT](LICENSE)
