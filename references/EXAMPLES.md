# Examples

Illustrative scenarios, not repository facts or default policies. The
shown evidence and user decisions apply only within each example.

## Review a Global File

Existing file:

```markdown
# Preferences

- Always answer in English.
- Never run git push.
- Use ripgrep when searching.
- Ask before every shell command that writes files.
```

No historical explanation is available. State that provenance is unknown;
current wording still establishes preferences and restrictions.

| Instruction | Verdict | Reason |
| --- | --- | --- |
| Always answer in English | KEEP | Clear language preference; short and useful without guessing its origin |
| Never run git push | KEEP | Explicit restriction; preserve it regardless of whether other tools can push |
| Use ripgrep when searching | KEEP | Specific tool preference; no evidence that it is obsolete or merely a speed hint |
| Ask before every shell command that writes files | REWRITE (pending approval) | Could block local tests that write caches while leaving editing tools uncovered; ask whether that distinction is intentional |

Question: should the last rule remain tool-specific, or should an agreed
scope of repository edits and local validation be allowed regardless of
tool? Explain the reduced approval overhead and reduced per-action control;
do not pick for the user.

A complete conservative replacement pending the answer preserves the
restriction:

```markdown
# Preferences

- Answer in English.
- Never run git push.
- Use ripgrep when searching.
- Ask before every shell command that writes files.
```

For an implementation workflow with no stopping criteria, separately offer
completion conditions for approval: finish after agreed checks pass;
report unresolved blockers and request decisions when needed. Do not insert
this new rule into the file before the user agrees. Keep questions outside
the proposed file. Unknown history alone does not require more interviews.

## Create a Project File

Read-only evidence: `package.json` specifies pnpm and defines `test` and
`typecheck`; CI runs both. The README describes a webhook delivery service.
Applicable parent instructions already define permissions. The user asks
for an English file and adds a conventional-commit preference.

Draft:

```markdown
# Acme Webhooks

A webhook delivery service.

- Package manager: pnpm
- Test: `pnpm test`
- Typecheck: `pnpm typecheck`
- Use conventional commits.
```

Explain that commands were found in configuration, not executed. Omit an
unsupported build command without starting another search or interview.
Do not duplicate inherited permissions. No review table or extra section
files are needed; await approval before writing the draft.

## Simplify an Existing Project File

The user wants a shorter file, not a new document hierarchy. Existing file:

```markdown
# Acme Webhooks

- Use pnpm.
- Run tests with `pnpm test`.
- The implementation is in `src/legacy/`.
- The loader currently uses zero workers.
- Write clean code.
```

Evidence: manifest and CI confirm pnpm and the test command. The directory
map is outdated; the named directory no longer exists. The worker count
matches the loader configuration, but merely repeats its current default;
no instruction to preserve that default or associated workflow hazard is
established. No origin records are available for the rules.

| Instruction | Verdict | Reason |
| --- | --- | --- |
| Use pnpm | KEEP | Confirmed by configuration; avoids choosing a different package manager and changing the lockfile |
| Run tests with `pnpm test` | KEEP | Confirmed by CI; identifies the intended test entrypoint without rediscovering it |
| Implementation is in `src/legacy/` | DROP | The named directory is absent, not merely a path that might someday drift |
| Loader currently uses zero workers | DROP | Accurate but duplicates a readily available default without an established decision benefit; not a request to change loader behavior |
| Write clean code | DROP | No concrete decision or completion criterion |

Complete replacement proposal:

```markdown
# Acme Webhooks

- Use pnpm.
- Run tests with `pnpm test`.
```

No material permission or boundary ambiguity was found; do not invent one
to fill the five dimensions. After approval, write and read back this
single file. No new linked documents or unrelated source edits are needed.
If evidence instead ties zero workers to a platform failure, that changes
its retention value: preserve the constraint with its reason rather than
mechanically deleting configuration details.

## Keep a Factual Review Bounded

Suppose a research project file describes a dual-branch classifier and names
two supported tasks. A targeted check confirms both, while configuration
also lists another task and additional branch switches. This does not by
itself make the overview wrong or require a catalog of every switch.
If the task list claims to be exhaustive, correct that claim using the
observed configuration. Do not call the extra task "exploratory" or
"secondary" without evidence of its role. Ask about positioning only if it
matters to the proposed instruction.

Once the relevant claims and retention decisions are supported, stop.
Delegate a specific unresolved claim if useful, not a search for every
important detail the file omits. Report static checks as static checks;
do not run training merely to review the instruction file.
