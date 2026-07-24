# Retrams Dependabot templates

Dependabot has no shared/central config, so these are **copy-adopted**: pick the
file matching your stack and copy it to your repo's `.github/dependabot.yml`.

| Template | Ecosystems |
|----------|-----------|
| `node.yml` | npm/yarn + github-actions + docker |
| `python-uv.yml` | uv + github-actions + docker |
| `actions-only.yml` | github-actions |

## Policy (all templates)

- **Monthly**, all updates for an ecosystem **grouped into one PR**.
- `open-pull-requests-limit: 3`.
- **7-day cooldown** (`cooldown: default-days: 7`) — Dependabot holds a freshly
  published version for a week before proposing it, a supply-chain buffer against
  compromised just-released packages. Also required by the org `zizmor` check
  (`dependabot-cooldown`), which fails a config that omits it.
- **Routine major bumps are ignored** (`version-update:semver-major`) — do those
  by hand.
- **Security fixes always land.** `update-types` scopes the ignore to *version*
  updates only; alert-driven Dependabot *security* updates still open a PR even
  when the only fix is a major bump (requires Dependabot security updates enabled
  for the repo/org). Do **not** replace this with a blanket version pin.
- **github-actions** refs stay SHA-pinned with a `# vX.Y.Z` comment; Dependabot
  bumps both the SHA and the comment in place.

## Adopt

1. Copy the matching template to `<repo>/.github/dependabot.yml`.
2. Remove any ecosystem block your repo doesn't have (no `Dockerfile` → drop the
   `docker` block, or use `actions-only.yml`).
3. Check for a config error: repo → Insights → Dependency graph → Dependabot.

## Docker + GitOps caveat

The `docker` ecosystem updates base-image `FROM` tags in the **Dockerfile**. It
must **not** manage the deployed app-image tag in **k8s manifests** — those are
owned by the Promote workflow + Argo CD. With `directory: "/"`, confirm Dependabot
only picks up the `Dockerfile`; if it proposes a k8s image-tag bump, add an
`ignore` for that app image (e.g.
`registry.digitalocean.com/the-retrams-registry/*`) in your repo's copy.
