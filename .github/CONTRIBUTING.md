# Contributing to Retrams repos

Org-wide conventions. These apply to every Retrams repository — the issue and PR
templates and this doc live in `Retrams-AS/.github` and propagate automatically
to any repo without its own.

## Referencing issues and PRs

Issue and PR **titles are plain summaries** — no ID prefix. GitHub's own number
is the identifier, and issues and PRs share one number sequence per repo.

- **Within a repo:** reference by `#12`.
- **Across repos:** prefix the number with the repo's short slug — e.g. `ADM #12`
  for adminpanel — or paste the full issue/PR URL. Either makes it unambiguous
  which repo's `#12` you mean; the slug covers both issues and PRs.

## Issue types

Pick the matching form when opening an issue:

- **Bug report** — Summary · Steps to reproduce · Expected · Actual/impact · Proposed fix (optional).
- **Feature request** — Problem/motivation · Proposed solution · Alternatives · Context.
- **Chore / audit finding** — Summary · Evidence (file:line) · Why it matters · Proposed remediation · Notes.

## Commits

Conventional Commits, with a scope:

```
type(scope): summary
```

- **`type`** — one of `feat` `fix` `docs` `chore` `ci` `refactor` `test` `release`.
- **`scope`** — the subsystem touched (`k8s`, `deps`, `readme`, `api`). Omit it only
  when the change genuinely spans the whole repo.
- **Summary** — imperative, no trailing period, and no issue-ID prefix (see
  *Referencing issues and PRs* above). The body explains *why*; the diff already
  shows *what*.

Branches use the same vocabulary: `type/slug` — `chore/krr-rightsize`,
`docs/readme-deploy-reality`, `release/2026-08.1-prod`.

Closing keywords (`Closes #12`) belong in the **PR body**, not in commit messages.

## Pull requests

Follow `PULL_REQUEST_TEMPLATE.md`: a summary, linked issues, the changes, how it
was verified, and the checklist. Link issues with a closing keyword and the
GitHub number — `Closes #12` — which fills the PR's Development section and
auto-closes the issue on merge (same-repo issues only; a number in another repo
won't auto-close). **Every PR is reviewed and verified by a human before merge —
the approval is that sign-off.**

## AI-assisted contributions

Agents and AI assistants are welcome, under two rules:

1. **Every AI-assisted commit carries an attestation** naming the harness, its
   version, the model and the effort level, as a trailer:

   ```
   Assisted-by: claude-desktop/2.1.293 claude-opus-5-5 effort=medium
   ```

   The line records what the harness observed, not what the agent believes about
   itself. In Claude Code the `retrams-contributing` plugin's hook writes it on
   every commit, reading the version, model and effort from the session, and
   replaces any attribution line the agent wrote. A harness without that hook
   writes it by hand in the same shape, `<harness>/<version> <model-id>` and
   `effort=<level>` when the harness has one; the `retrams-contributing:git-commit`
   skill has the field-by-field spec. A field that cannot be read means no line,
   never a guess.

   It is an attestation, not co-authorship, so there is no
   `Co-Authored-By … <noreply@anthropic.com>` line any more.

   **CI checks the format.** `commit-trailer-check.yml` fails any PR with a
   malformed `Assisted-by` line, and warns on an old Anthropic co-author line
   until the switch is done. It cannot check that a line is *present*: nothing
   observable from CI reveals that a commit was AI-assisted, so presence is the
   contributor's responsibility.

2. **A human reviews and verifies all of it before merge.** AI authorship never
   substitutes for review — the contributor is accountable for the correctness
   and licensing of everything they submit.

## Creating issues/PRs programmatically (agents, `gh`, API)

GitHub applies these templates only in the **web UI**. The REST API, the GitHub
MCP tools, and `gh --body` do **not** apply them. When creating issues or PRs by
automation, **mirror the matching template by hand**: fill the same sections as
the relevant issue form, and structure PR bodies to match
`PULL_REQUEST_TEMPLATE.md`.

An issue form's `labels:` are not applied either — pass them explicitly
(`gh issue create --label bug`), or the issue lands untyped.

The `retrams-contributing` plugin in `Retrams-AS/agent-plugins` encodes this for
Claude Code: it fetches these templates at use-time and mirrors them. This doc
remains the human- and agent-readable source.
