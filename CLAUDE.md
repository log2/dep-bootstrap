# dep-bootstrap — development guide

## Purpose

Bootstrap script that installs and initializes the `dep` package manager (built on basher). Used as the entrypoint for setting up the `play/` library ecosystem on a new machine.

## How it works

1. `dep-bootstrap.sh <version>` — downloads a pinned version of `bootstrap.sh` from this repo, caches it under `~/.dep/bootstrap/<version>/`, and sources it
2. `bootstrap.sh` — the actual dep runtime: provides `dep()`, `dep_include()`, `dep_define()`, which clone tagged git repos from GitHub and link them via basher
3. Consumers call `dep include <org>/<repo> <module>` to load library files

## Scripts

| File | Purpose |
|------|---------|
| `dep-bootstrap.sh` | Thin wrapper: fetch+cache versioned `bootstrap.sh`, source it |
| `bootstrap.sh` | dep runtime: define/include with basher linking |
| `samples/vanilla.sh` | Minimal usage example |
| `samples/import-sample.sh` | More complex example with multiple deps |

## Local development

Set `local-SNAPSHOT` in your `.envrc` to point dep at local checkouts instead of GitHub:

```bash
export local_SNAPSHOT="$(dirname "$(pwd)")/envrc-framework"  # for envrc-framework
# dep-bootstrap itself uses SNAPSHOT via the DEP_REPO_BASE_URL env var
```

The `bootstrap.sh` checks for `local-SNAPSHOT` tag and uses a symlink in `~/.dep/repository/` instead of cloning.

## Releasing

1. Bump `scriptVersion` in `bootstrap.sh`
2. Commit and push
3. `git tag <version> && git push origin <version>`
4. Update all consumer `dep-bootstrap.sh` invocations to the new tag

## Pre-commit

- `shfmt -i 4 -bn -ci -fn`
- `shellcheck --severity=warning`
