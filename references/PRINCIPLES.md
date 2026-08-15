# Principles: Minimalism and Progressive Disclosure

Evidence for why AGENTS.md should be small and point elsewhere. Read this when judging what belongs in the root file or when the user pushes back on trimming.

## The Instruction Budget

- Frontier LLMs follow ~150–200 instructions with reasonable consistency; smaller models and non-thinking models follow fewer. (Kyle, Humanlayer)
- Every token in AGENTS.md loads on **every request**, relevant or not.
- A 2026 evaluation of repository context files found they often raised inference cost by more than 20%, and recommends retaining only minimal, non-redundant requirements. (arXiv:2602.11988)

Consequences: a small focused file leaves more tokens for the actual work; a bloated file wastes tokens and confuses the agent. Irrelevant instructions are token waste plus distraction.

## Stale Documentation Poisons Context

- Humans tolerate stale docs because they have built-in memory to be skeptical of them. Agents re-read them on every request — stale information actively poisons their context.
- File paths change constantly. "Authentication lives in `src/auth/handlers.ts`" goes wrong the moment the file is renamed or moved, and the agent will confidently look in the wrong place.
- **Describe capabilities, not structure.** Give hints about where things *might* be and the overall shape of the project. Let the agent generate its own just-in-time documentation during planning.
- Domain concepts ("organization" vs "group" vs "workspace") are stabler than paths, so they are safer to document — but they can still drift in fast-moving AI-assisted codebases. Keep a light touch.

## The Minimum Root File

Absolute minimum for the root AGENTS.md:

1. One-sentence project description (acts like a role-based prompt)
2. Package manager (if not npm; `corepack` also silences the warnings)
3. Build/typecheck commands (if non-standard)

That is it. Everything else goes elsewhere.

## Progressive Disclosure

Give the agent only what it needs right now and point to resources when needed. Agents are fast at navigating documentation hierarchies.

- Move language rules out of the root file: root says "For TypeScript conventions, see docs/TYPESCRIPT.md" — a conversational reference, no "always", no all-caps forcing.
- Nest further: `docs/TYPESCRIPT.md` can reference `docs/TESTING.md`, which references the test runner. Build a discoverable resource tree:

  ```text
  docs/
  ├── TYPESCRIPT.md   # references TESTING.md
  ├── TESTING.md      # references specific test runners
  └── BUILD.md        # references esbuild configuration
  ```

- External links are fine (framework docs, Prisma docs, etc.).
- Agent skills are another form of progressive disclosure.

Benefits: domain rules load only when that domain is touched; other tasks don't waste tokens; the file stays focused and portable across model changes.

## Multi-Level AGENTS.md

Subdirectory AGENTS.md files merge with the root — powerful for monorepos:

| Level | Content |
| --- | --- |
| Root | Repo purpose, how to navigate packages, shared tools |
| Package | Package purpose, specific stack, package-specific conventions |

Don't overload any level: the agent sees all merged files in its context. Keep each level focused on its own scope.

## Refactoring Checklist (existing files)

1. Find contradictions — for each, ask the user which version to keep
2. Extract the essentials (the three items above)
3. Group the rest into logical category files (TypeScript conventions, testing, API design, git workflow)
4. Create the structure: minimal root + linked files + suggested docs/ layout
5. Flag for deletion: redundant (the agent already knows this), too vague to be actionable, or overly obvious ("write clean code")
