# dep-bootstrap — status

## Health: stable, low activity

- bootstrap.sh v0.5.5 — has been stable for a long time
- No known bugs in the dep runtime

## Known limitations

- `dep` relies on GitHub at runtime for cloning: no network = no dep
- basher is the underlying package manager; it adds `~/.basher/` to PATH which is non-standard
- No lockfile: tags are pinned per-call but there's no global lockfile concept
- `_DEP_VERBOSENESS_LEVEL=0` silences all output in production use; debugging requires setting it to 3-4

## Tech debt

- Long-term: replace dep/basher with a simpler local-path sourcing mechanism (item 6 in owncluster roadmap)
- `dep-bootstrap.sh` clones this repo at startup via git; DNS failure breaks the entire `.envrc` load

## Dependencies

None — this is the bootstrapper. Loaded by all other repos via:
```bash
. "$_DEP_BOOTSTRAP" 0.5.5
```
