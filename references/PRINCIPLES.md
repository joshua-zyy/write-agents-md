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
- External links are fine (framework docs, API references), and agent
  skills are another form of progressive disclosure.

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
