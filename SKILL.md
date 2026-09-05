---
name: write-agents-md
description: >-
  Guides users to create or review AGENTS.md and related agent instruction
  files (CLAUDE.md, GEMINI.md) at global, project, or nested scope. Combines
  read-only evidence gathering with focused questions to produce minimal
  instruction files with explicit permissions and completion conditions.
  Use when the user asks to write, create, generate, refactor, review,
  audit, improve, or clean up AGENTS.md, CLAUDE.md, GEMINI.md, or other
  agent instruction files.
---

# Write AGENTS.md

Create or review small, focused agent instruction files. This is a
guided-writing skill, not an auto-generator: gather evidence read-only, ask
only what the evidence cannot answer, and leave every permission and
deletion decision with the user.

Talk to the user in the language they use. Instruction files are written in
the language the user prefers for those files — this may differ from the
conversation language. Follow the stated preference; ask only if it is
unknown and it matters.

## Why Small

Loaded instructions consume context even when irrelevant to the current
task; contradictory rules can mislead. Loading scope varies by tool.
See [PRINCIPLES.md](references/PRINCIPLES.md) for the reasoning.

## Establish Scope

Use the request and available context to establish:

- **Scope**: global (user-level preferences and standing permissions),
  project (repository root), or nested (subdirectory or package).
- **Mode**: create a new file, or review an existing one.
- **Target file(s)**: which file(s) will be written or edited.
- **File language**: the language the user wants those files in.

Ask only about what is unknown *and* changes the output or touches
permissions. If the request plus evidence already answers it, proceed —
never run a full interview by default.

Read only what the scope needs:

- **Global**: the existing file, if reviewing. Global instructions are the
  user's own preferences and permissions — do not scan repositories to
  source them; ask the user instead.
- **Project / nested**: applicable parent and existing instruction files,
  relevant README, manifests, CI config, and directory layout. Check inherited
  constraints before adding duplicate or conflicting rules. Stop once the
  evidence supports the decisions at hand; do not read the full codebase.

## Create Path (new file)

Build the file from exactly two sources:

1. **Verifiable facts** — commands and layout observed in manifests, CI, or
   scripts; cite what you saw ("package.json shows pnpm").
2. **Explicit user preferences** — stated in this conversation or in
   applicable instruction files. Do not copy permissions or preferences
   from unrelated scopes.

Never invent a command, preference, permission, or approval policy because
a template has a slot for it. Raise missing authorization or completion
conditions only when they affect the intended workflow; offer options, not
silent defaults. An absent command does not by itself require a question.
A fresh create needs no KEEP/DROP table; there is nothing prior to preserve.

## Review Path (existing file)

Produce two artifacts, in order.

**1. Decision table** — one row per instruction in the existing file, no
omissions:

| Instruction | Verdict | Reason |
| --- | --- | --- |
| (every instruction) | KEEP / DROP / REWRITE | one line |

Assess each row along five dimensions and surface the evidence that
matters:

- **Provenance** — separate what you can verify (repo docs or CI, an
  explicit user statement) from what you infer (probable leftover, stale
  copy). Unknown history is stated as unknown; do not dig through old
  sessions indefinitely to reconstruct it. Assess the weakness the rule
  currently guards against, whether it still occurs, and any side effects;
  distinguish this assessment from historical intent. Do not claim newer
  models no longer need a rule without evidence.
- **Boundary strength** — could absolute wording halt work the user wants
  continued? Examine effects, scope, and exceptions, not just tool names.
  Location or tool restrictions may be intentional; do not replace them
  without confirmation. Ask about genuine ambiguities, such as whether a
  test that writes cache files needs approval.
- **Missing permissions** — what the agent may need to do that nothing
  authorizes or forbids. Offer these as options to confirm; never adopt
  permissions the user has not given, and never assume the user wants
  standing permissions you have seen elsewhere.
- **Missing completion conditions** — is it clear when to continue, finish,
  or seek human review? Where relevant, propose bounded verification and
  honest reporting of unresolved blockers rather than endless retries;
  do not treat every recoverable failure as a reason to stop.
- **Context economics** — what the instruction costs versus what it buys
  whenever loaded. Vague or redundant rules are DROP
  candidates, not automatic deletions. Resolve conflicting intent with the
  user; do not discard a specific preference merely as "model knowledge".

**2. Complete replacement draft** — the full file after applying the
table; every original instruction accounted for as kept, rewritten, or
dropped pending the user's confirmation. When a rule's intent or
authorization is unclear, ask directly. Keep open questions outside the
proposed file and preserve the existing boundary in a conservative draft;
do not replace an active restriction with a pending marker.

## Apply and Verify

Respect review-only requests. Show the draft (plus the decision table for
reviews) before applying changes; use a diff when helpful. Write only with
explicit approval. Once a specific change is approved, do not re-ask.

Before delivery, check that intent, scope, and permissions match the evidence
or approved decisions, commands are not invented, file language is correct,
and local references resolve. Remove template placeholders from final files;
do not apply unresolved permission changes. If writing, read the files back
and repeat these checks. State anything unverified; distinguish a command
found in configuration from one actually tested. End at the requested
proposal or applied change, without extra investigation once checks pass.

## Output Shape

- Templates are optional: use [the project skeleton](assets/templates/root-AGENTS.md)
  only for project files and [the section skeleton](assets/templates/section.md)
  only when splitting is useful. Take what fits the scope, drop the rest.
  No fixed line count, required section set, blanket ban on paths, or
  mandated link tree; when linking, point directly at the file
  that holds the answer instead of building chains of pointers.
- Split into separate files only when the content earns it; a single small
  file is fine, and stable load-bearing paths may stay if the user wants
  them.
- No generic engineering precepts ("write clean code") and no heavyweight
  process frameworks.
- Cross-tool adaptation (symlink / import / flag) is offered only if the
  user works with other tools and wants it — see
  [TOOL-CONVENTIONS.md](references/TOOL-CONVENTIONS.md).

## References

- [PRINCIPLES.md](references/PRINCIPLES.md) — why instruction files stay small; read when judging what to keep or when the user pushes back on trimming
- [TOOL-CONVENTIONS.md](references/TOOL-CONVENTIONS.md) — per-tool file names and loading behavior, an unverified starting point; read when adapting output for a specific platform
- [EXAMPLES.md](references/EXAMPLES.md) — short worked examples: global review, project create, simplification; read when drafting
