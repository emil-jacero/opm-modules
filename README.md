# opm-modules

Personal [OPM](https://opmodel.dev) module fleet: media and GPU-transcoding applications as CUE
modules, published as `jacero.se/modules/<name>@v1` to `ghcr.io/emil-jacero`.

Migrated in 2026-09 from [open-platform-model/modules](https://github.com/open-platform-model/modules),
whose `opmodel.dev/modules/*` namespace carries business and enterprise modules only. Every module
here starts its own line at `1.0.0`.

## Consuming a module

Map the domain before anything resolves; an unmapped domain falls through to `registry.cue.works`:

```sh
export CUE_REGISTRY='jacero.se=ghcr.io/emil-jacero,opmodel.dev=ghcr.io/open-platform-model,registry.cue.works'
export OPM_REGISTRY="$CUE_REGISTRY"
cue mod get jacero.se/modules/jellyfin@v1
```

The same mapping goes into an opm-operator `--registry` flag or `~/.opm/config.cue`.

## Module anatomy

```text
<module-name>/
  cue.mod/module.cue      CUE module manifest — jacero.se/modules/<name>@v1
  identity/identity.cue   committed identity package (ModulePath + Version)
  module.cue              module metadata + #config schema
  components.cue          component definitions (catalog blueprints/traits/resources)
  README.md               usage, architecture, quick start
  DEPLOYMENT_NOTES.md     issues and fixes found during deployment
```

## Working here

- `AGENTS.md` — repo working rules (read first).
- `DESIGN_PATTERNS.md` — reusable CUE patterns across modules.
- `Taskfile.yml` — `task fmt` / `task vet` / `task tidy` / `task check`. There is no publish
  task: publishing is CI's, through `opm module publish` on every push to `main`.
- `release-please-config.json` / `.release-please-manifest.json` — per-module version decisions.
  release-please writes the manifest and the changelogs; `opm module version set` is the only
  writer of a module's `identity/identity.cue`.
