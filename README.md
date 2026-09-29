# plugin-sidecar

The `sidecar` plugin kind for OpenCharly — decode of the authored `sidecar:`
entity.

`plugin-sidecar` is a **kind provider** that dispatches via the
`pb Invoke(OpLoad)` envelope: it decodes the authored `sidecar:` node into its
core spec type and re-marshals it as canonical JSON. It serves itself in **both**
placements (compiled-in or out-of-process) — kinds are pb-shape, so no kit
contract is needed.

## What it provides

| Capability | Surface |
|---|---|
| `kind:sidecar` | decode of the authored `sidecar:` entity (the `sidecar:` kind) |

## How to use it

Compose the plugin candy where a box or deploy authors a `sidecar:` node:

```yaml
- '@github.com/opencharly/plugin-sidecar/candy/plugin-sidecar:<tag>'
```

## Layout

- `candy/plugin-sidecar/` — the plugin module: `plugin.go` (the kind provider +
  `NewMeta()`), `resolve.go` (the entity decode), `schema/sidecar.cue`,
  `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-automation:sidecar` — the sidecar-container model and
  pod networking this kind decodes. This candy carries no `skill:` entity of its
  own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
