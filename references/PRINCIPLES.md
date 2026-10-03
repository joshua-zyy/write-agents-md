# Principles: Why Instruction Files Stay Small

The reasoning behind minimalism and progressive disclosure. Read this when
judging what belongs in an instruction file, or when the user pushes back
on trimming.

## Context Is a Budget

- Instructions consume context whenever loaded, whether or not they help
  the current task. Loading scope and frequency vary by tool and setup.
- Irrelevant instructions are pure cost; contradictory ones are worse —
  the agent must guess which one wins.
- The tradeoff is qualitative: a larger loaded file can spend more context
  on rules unrelated to the task. Judge each candidate rule by what it
  buys against that cost. No fixed token or rule count
  is "correct" — the right size depends on what the user actually needs
  loaded every time.
- Measure the delta, not the absolute size. Comparing revisions shows which
  change added what and where a file grew; a constraint restated in two
  rules is the first DROP candidate. Numbers locate the problem; they are
  not the goal.

## Stale Content Poisons Context

- Stale instructions can mislead an agent even when they look authoritative.
  Check important claims against current evidence rather than assuming
  either humans or models reliably recognize outdated content.
- File paths drift fast. "Authentication lives in `src/auth/handlers.ts`"
  is wrong the moment the file moves, and the agent will confidently look
  in the wrong place.
- Prefer capabilities and domain concepts ("organization" vs "group" vs
  "workspace") over brittle paths — but a stable, load-bearing path the
  user genuinely wants is fine. Keep a light touch either way.

## Progressive Disclosure

Keep useful rules at the scope where they apply. Moving detail out can
reduce loaded context but adds navigation and maintenance costs.

- Global files hold user-wide preferences and boundaries. Project and
  nested files hold applicable project facts and rules, including useful
  conditional rules; there is no required set of sections.
- Keep a small file together. Split substantial specialized material only
  when selective loading is useful and supported; prefer existing documents
  over creating new ones solely to shorten the root file.
- Link directly to the file that holds the answer. Chains of pointers
  ("see A, which says see B") add hops without adding information; nest
  only when a file genuinely serves more than one audience.
- External links are fine (framework docs, API references).

## Choosing the Carrier

A rule can live in the global instruction file, a project instruction file,
or an agent skill. The deciding question is *when the rule must apply*,
not how long the rule is.

| Carrier | Holds | Context cost |
| --- | --- | --- |
| Global instruction file | User-wide preferences, permissions, safety boundaries, short unconditional constraints | Loaded in every session, including non-code tasks |
| Project instruction file | Repo facts: commands, paths, test tiers, gates, conventions | Loaded whenever the agent works in that repo |
| Agent skill | Procedural detail only some tasks need: audit methods, cleanup procedures, tool recipes | One description line until a task matches; the body loads on match |

- Move procedural detail into a skill when most tasks never need it. This is
  the cheapest way to shrink a global file without losing the rule.
- Never move permissions, safety boundaries, or anything that must hold on
  every task. A skill that fails to match is indistinguishable from no rule,
  and platforms that advertise skill descriptions only advertise them.
- Keep a one-line pointer in the file whose tasks would need the skill when
  the platform does not reliably surface skill descriptions.
- A rule that applies to every task in every repo — the user's language,
  output shape, approval boundaries — stays in the global file even when it
  is long; a repo-specific command does not, even when it is short.
- Skills are not the only option: an existing document, README section, or
  project file may already be the right home. Prefer a carrier that exists.

## Multi-Level Instruction Files

Subdirectory instruction files can merge with the root — useful for
monorepos. Whether merging happens, and how, varies by tool and version;
see [TOOL-CONVENTIONS.md](TOOL-CONVENTIONS.md) before promising merge
behavior.

| Level | Content |
| --- | --- |
| Root | Repo purpose, how to navigate packages, shared tools |
| Package | Package purpose, specific stack, package-specific conventions |

Where levels merge, every level competes for the same context — keep each
level focused on its own scope.
