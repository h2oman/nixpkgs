# h2oman/nixpkgs automation branch

This branch is the **default branch** of this fork but contains only automation
metadata for `h2oman`'s downstream consumption of `claude-code` from nixpkgs.

It exists to satisfy a GitHub Actions constraint: scheduled (`schedule:`)
workflows only fire from a repository's default branch. This branch decouples
the scheduling surface from `claude-code-latest`, the branch this fork's
downstream consumer (`h2oman/nixos-playground`) actually pulls `claude-code`
from via flake input.

## Branches in this fork

- **`automation`** (this branch, default) — automation metadata only. `.github/workflows/*` for downstream-consumption automation. Created from an empty tree, no upstream history.
- **`master`** — fast-forward mirror of `NixOS/nixpkgs@master`. Synced on demand; never carries divergent commits.
- **`claude-code-latest`** — the branch downstream consumes via flake input. Usually fast-forwarded from `master`; may carry ahead-of-upstream commits for `claude-code` manifest bumps.
- **`claude-code-latest-legacy`** — pre-2026-04-20 `buildNpmPackage`-era history, preserved for provenance.

## What runs here

- `.github/workflows/claude-code-auto-bump.yml` — fires on cron (Mon–Fri 20:00 MT) and `workflow_dispatch`. Checks `claude-code-latest`'s `manifest.json` against Anthropic's `latest` pointer; if drifted, fetches the new manifest, sanity-checks one platform's binary hash, and commits the update to `claude-code-latest`.

This branch holds **no outbound credential**. Only `GITHUB_TOKEN`, scoped to this repo.

For full context see [h2oman/nixos-playground#249](https://github.com/h2oman/nixos-playground/issues/249).
