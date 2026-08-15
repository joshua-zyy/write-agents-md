# write-agents-md

[English](README.md) | [中文](README.zh-CN.md)

An agent skill for writing, refactoring, or slimming down **AGENTS.md** and related agent instruction files (CLAUDE.md, GEMINI.md).

This is a **guided-writing workflow, not an auto-generator**: it gathers repository evidence read-only, interviews the user for preferences, and produces a minimal root file that points to progressively disclosed detail.

## Core Principles

Every token in AGENTS.md is loaded on every request, relevant or not — so smaller is better:

1. **Instruction budget** — frontier LLMs reliably follow only ~150–200 instructions; bloated files waste tokens and confuse the agent.
2. **The root file holds three things only** — a one-sentence project description, the package manager (if not npm), and non-standard build/typecheck commands.
3. **Progressive disclosure** — everything else lives in separate files referenced by links (e.g. `docs/TYPESCRIPT.md`).
4. **Describe capabilities, not paths** — file paths go stale and poison agent context.
5. **Never auto-generate** — generated files prioritize comprehensiveness over restraint. Keep the human in the loop.

## Workflow

- **Phase 1: Gather evidence (read-only)** — survey the README, package manifests, directory structure, existing instruction files, and CI config; extract facts without reading the full codebase.
- **Phase 2: Interactive interview** — ask at most 3–4 questions per message; never ask what the evidence already answers.
- **Phase 3: Produce output** — generate a minimal root file from the template, split domain rules into linked section files, flag redundant rules for deletion, and offer cross-tool adaptation (symlink / import / launch flag).

## Installation

Copy the whole directory into your agent's skills folder, for example:

- **Pi**: `~/.pi/agent/skills/write-agents-md/`
- **Claude Code**: `~/.claude/skills/write-agents-md/`

Then trigger it by asking your agent to "write an AGENTS.md" or "slim down CLAUDE.md".

## Project Structure

```text
write-agents-md/
├── SKILL.md                        # Skill entry: principles, workflow, output quality checklist
├── assets/templates/               # Output templates
│   ├── root-AGENTS.md              #   Minimal root file template
│   └── section.md                  #   Domain-rule section template
└── references/                     # On-demand reference material
    ├── PRINCIPLES.md               #   Why minimalism and progressive disclosure
    ├── TOOL-CONVENTIONS.md         #   How each agent platform loads instruction files
    └── EXAMPLES.md                 #   Good/bad examples and multi-level layouts
```

## License

[MIT](LICENSE)
