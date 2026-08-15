---
name: write-agents-md
description: >-
  Guides users to write, refactor, or slim down AGENTS.md and related agent
  instruction files (CLAUDE.md, GEMINI.md). Combines repository evidence
  gathering with an interactive interview to produce a minimal,
  progressively-disclosed instruction file instead of a bloated auto-generated
  one. Use when the user asks to write, create, generate, refactor, improve,
  or clean up AGENTS.md, CLAUDE.md, GEMINI.md, or other agent instruction files.
---

# Write AGENTS.md

Create small, focused AGENTS.md files. This skill is a guided-writing workflow, not an auto-generator: read the repository for evidence, interview the user for preferences, and produce a minimal root file that points to progressively disclosed detail.

Talk to the user in the language they use. All output files (AGENTS.md etc.) are written in the user's language unless they prefer English.

## Core Principles

Every token in AGENTS.md is loaded on every request, relevant or not. The ideal file is as small as possible.

1. **Instruction budget** — frontier LLMs follow ~150–200 instructions reliably. Bloated files waste tokens and confuse the agent.
2. **Root file holds three things only** — one-sentence project description, package manager (if not npm), non-standard build/typecheck commands.
3. **Progressive disclosure** — everything else lives in separate files referenced by links (e.g. `docs/TYPESCRIPT.md`). Agents navigate doc hierarchies efficiently.
4. **Describe capabilities, not paths** — file paths go stale and poison context. Domain concepts are safer.
5. **Never auto-generate** — generated files prioritize comprehensiveness over restraint. Keep the human in the loop.

## Workflow

### Phase 1: Gather Evidence (read-only)

Survey the repository before asking anything:

- README, package manifests (package.json, pyproject.toml, Cargo.toml, ...), lockfiles
- Directory structure at root and 1–2 levels deep
- Existing instruction files (AGENTS.md, CLAUDE.md, GEMINI.md, .cursorrules, docs/)
- CI config (.github/workflows, Makefile, scripts/)

Extract facts: package manager, build/test/typecheck commands, monorepo layout, languages. Do not read the full codebase.

### Phase 2: Interactive Interview

Ask in small batches — at most 3–4 questions per message. Never ask what the evidence already answers.

Priority order:

1. One-sentence project description (what it is, why it exists)
2. Commands and package manager (confirm or correct the evidence)
3. Personal preferences: commit style, naming, boundaries, explicit no-gos
4. Ambiguities the evidence could not resolve

When refactoring an existing file: for each contradiction, ask the user which version to keep. Do not delete anything without confirmation.

### Phase 3: Produce Output

1. Write the minimal root AGENTS.md from [root-AGENTS.md](assets/templates/root-AGENTS.md)
2. Split domain rules into separate files from [section.md](assets/templates/section.md) and link them from the root file
3. Flag deletions: redundant, too vague, or overly obvious rules ("write clean code")
4. Cross-tool adaptation: if the user works with Claude Code, Gemini CLI, Cursor, or Aider, offer the adaptation (symlink / import / flag) from [TOOL-CONVENTIONS.md](references/TOOL-CONVENTIONS.md)
5. Offer to write the file(s) into the repository; never overwrite an existing file without showing the diff and asking first

## Output Quality Checklist

For every rule in the root file, ask:

- Relevant to **every** task in the repo? If not → separate file or drop.
- Actionable and specific? If vague → rewrite or drop.
- Redundant or obvious to the model? → drop.
- References a path or structure? → rephrase as a capability or hint.
- Contradicts another rule? → resolve with the user first.

## References

- [PRINCIPLES.md](references/PRINCIPLES.md) — why minimalism and progressive disclosure matter; read when judging what to keep or when the user pushes back on trimming
- [TOOL-CONVENTIONS.md](references/TOOL-CONVENTIONS.md) — per-tool naming and loading behavior; read when adapting the output for a specific agent platform
- [EXAMPLES.md](references/EXAMPLES.md) — good/bad examples and multi-level layouts; read when drafting or when the user asks for examples
