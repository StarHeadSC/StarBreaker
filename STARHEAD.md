# StarHead fork of StarBreaker

This fork exists for StarHead's game data export (repository `star-head-data-export`). It is
upstream [diogotr7/StarBreaker](https://github.com/diogotr7/StarBreaker) plus the fixes below,
until upstream has them.

## Branches and tags

| Name | What it is |
| --- | --- |
| `main` | upstream `main`, unchanged |
| `starhead` | the release line: upstream plus the fixes below, this file and the trimmed release workflow |
| `fix/*` | one branch per fix, on top of upstream, meant for an upstream PR |
| `v<upstream version>-sh.<n>` | a release: the tag runs `.github/workflows/release.yml` |

## Fixes

| Fix | Branch | Upstream PR |
| --- | --- | --- |
| A CDF whose `Model` is a `.cga` body loads that body as its base mesh. Before, such weapons exported as "has no attachments" or as only the bullet. | `fix/cdf-cga-base-model` | not opened yet |

## Release

A tag `v*-sh.*` on `starhead` builds `starbreaker.exe` on `windows-latest` and publishes three files:

- `starbreaker-cli-<tag>-windows-x86_64.zip`: `starbreaker.exe`, `LICENSE`, `BUILD-INFO.txt`
- `starbreaker-blender-addon-<tag>.zip`: `starbreaker_addon/`, `LICENSE`, `BUILD-INFO.txt`
- `SHA256SUMS.txt`

The exporter pins one tag and its checksums. Its setup downloads both zips, so nobody needs Rust.
Upstream's Tauri desktop app is not built here.

The version inside the binary stays the upstream one (`starbreaker --version` prints
`0.3.2`). The exporter puts the tool versions into its render and bake keys, so a different
version string would change every key, and `--changed` or a plain bake run would redo every image
and model. `BUILD-INFO.txt` names the tag and commit.

## Updating

1. Merge upstream `main` into `starhead` (and into each open `fix/*` branch).
2. Drop a fix once upstream has it.
3. Tag the next `v<upstream version>-sh.<n>` on `starhead`.
4. Update the exporter's tool lock to the new tag and checksums.
