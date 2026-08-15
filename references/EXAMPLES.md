# Examples

Good and bad AGENTS.md content, plus multi-level layouts. Read this when drafting or when the user asks for examples.

## Bad: The Ball of Mud

```markdown
# Project

This is a web app with a React frontend, a Node backend, a Python data pipeline,
and a mobile app. We use pnpm. The backend is in server/src/, the frontend is in
web/src/, the mobile app is in mobile/, and the data pipeline is in pipeline/.
Authentication is in server/src/auth/handlers.ts and the database layer is in
server/src/db/repository.ts. We use PostgreSQL 16 with Prisma...

ALWAYS use const instead of let.
NEVER use var.
ALWAYS use TypeScript strict mode.
NEVER use any.
Use interface instead of type when possible.
Always write clean code.
Always run tests before committing.
Always follow the 12-factor app principles.
...
```

Problems: stale paths the agent will trust blindly, redundant rules ("clean code"),
contradictory piles of accumulated opinions, and token waste on every single request.

## Good: Minimal Root

```markdown
# Acme Dashboard

A web dashboard for Acme's internal analytics, React + Node + Postgres.

## Commands

- Package manager: pnpm
- Dev: `pnpm dev`
- Test: `pnpm test`
- Typecheck: `pnpm typecheck`

## Conventions

- For TypeScript conventions, see [docs/TYPESCRIPT.md](docs/TYPESCRIPT.md)
- For testing patterns, see [docs/TESTING.md](docs/TESTING.md)
```

Note the light touch: no "always", no all-caps forcing, just conversational references.

## Progressive Disclosure Tree

```text
docs/
├── TYPESCRIPT.md   # references TESTING.md
├── TESTING.md      # references specific test runners
└── BUILD.md        # references esbuild configuration
```

## Multi-Level (Monorepo)

Root `AGENTS.md`:

```markdown
# Acme Monorepo

A monorepo containing web services and CLI tools.

Use pnpm workspaces to manage dependencies.

See each package's AGENTS.md for specific guidelines.
```

Package `packages/api/AGENTS.md`:

```markdown
# API Package

A Node.js GraphQL API using Prisma.

Follow docs/API_CONVENTIONS.md for API design patterns.
```

Each level stays focused on what is relevant at that scope — the agent sees all
merged files, so overloading any level hurts everyone.

## Before / After Refactor

Before (root file, 120 lines of accumulated rules):

```markdown
- TypeScript: use interface not type, strict mode, no any, const not let...
- Testing: use vitest, describe/it, no snapshots, mock the db with...
- Git: conventional commits, scope in subject, no merge commits...
- API: REST over GraphQL, paginate everything, return 404 not 204...
```

After (minimal root + links):

```markdown
# Acme Dashboard

A web dashboard for Acme's internal analytics, React + Node + Postgres.

## Commands

- Package manager: pnpm
- Test: `pnpm test`

## Conventions

- For TypeScript conventions, see [docs/TYPESCRIPT.md](docs/TYPESCRIPT.md)
- For testing patterns, see [docs/TESTING.md](docs/TESTING.md)
- For Git workflow, see [docs/GIT.md](docs/GIT.md)
- For API design, see [docs/API.md](docs/API.md)
```
