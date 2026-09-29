# AGENTS.md — plugin-sidecar

Standalone plugin repo for the `sidecar` structural kind (`kind:sidecar`). The
plugin is a Go module at `candy/plugin-sidecar/` (module path
`github.com/opencharly/plugin-sidecar/candy/plugin-sidecar`); the root
`charly.yml` only declares `discover: candy` so the repo is a project and its
candy is scanned.

Canonical files:

- `candy/plugin-sidecar/charly.yml` — the `plugin-sidecar:` candy entity
  (`plugin:` block, `plan:` check).
- `candy/plugin-sidecar/plugin.go` — the kind provider (`Invoke(OpLoad)` +
  `NewMeta()`).
- `candy/plugin-sidecar/resolve.go` — the authored `sidecar:` entity decode.
- `candy/plugin-sidecar/schema/sidecar.cue` — the self-contained schema.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, the kind-decode seam, the per-plugin
  CUE-schema contract, placement. Load before touching the provider or schema.
- `/charly-automation:sidecar` — the sidecar-container model and pod networking
  this kind decodes.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-sidecar/` — compile the plugin module.
- `go test ./...` in `candy/plugin-sidecar/` — the plugin's Go tests
  (`resolve_test.go`).
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- There is no dedicated live bed: the entity decode is exercised by any
  box/deploy composing a `sidecar:` node.

## Modify this repo

- Edit the `plugin-sidecar:` candy entity, the Go source, and
  `schema/sidecar.cue` **together** — the schema is the single source for the
  `params/` struct, so a field change not mirrored in the schema desyncs the
  generated types.
- The provider serves **both** placements (compiled-in or out-of-process); do not
  describe it as one or the other.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
