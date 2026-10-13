# renovate-config

Shared [Renovate](https://docs.renovatebot.com) preset for `yoshintame/*` repositories, run by the Mend Renovate App.

## Usage

`renovate.json` of a repository:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>yoshintame/renovate-config#v1.0.0"]
}
```

The onboarding PR of a new repository suggests this preset by itself. The tag is pinned: Renovate opens a PR when a new version of the preset is released.

## What the preset does

- **Base:** `config:best-practices`, which pins GitHub Actions and reusable workflows to commit SHAs and devDependencies to exact versions.
- **Schedule:** the 1st and 3rd Monday of the month, 00:00–04:00 Asia/Bangkok; a version must be 7 days old. Security updates skip both.
- **Batches:** every non-major update in one PR; majors wait for approval in the Dependency Dashboard.
- **Own releases:** `yoshintame/**` and `@yoshintame/**` arrive at once, without cooldown, in one PR.
- **hk:** the version in `mise.toml` and the package URLs in `hk.pkl` move in one PR.
- **No lock file maintenance:** it would bypass the minimum release age.

## What it updates

| Where | How |
|---|---|
| `package.json`, `bun.lock` | built-in `bun` manager |
| `mise.toml` | built-in `mise` manager; exact versions only, `latest` and `lts` move only in `mise.lock` |
| `uses:` of actions and reusable workflows | built-in `github-actions` manager, SHA with a version comment |
| `package://github.com/<owner>/<repo>/releases/download/v<version>/<name>@<version>` in `*.pkl` | regex manager, GitHub releases |
| a value under `# renovate: datasource=<datasource> depName=<name>` in workflow YAML | regex manager |
| `flake.lock` | not updated: Nix inputs are refreshed by hand |

## Checks

`mise install && bun install && hk check --all` runs jactionlint and `renovate-config-validator --strict` on this repository. Releases are `vX.Y.Z` tags cut by `release-semantic.yml` of [yoshintame/workflows](https://github.com/yoshintame/workflows).
