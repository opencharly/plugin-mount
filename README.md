# plugin-mount

The `mount` state-provision verb for OpenCharly — probe and provision a
filesystem mount.

The plugin is a host-coupled verb on the SDK/kit contract
(`CheckVerbProvider` + `ProvisionActor`), so it is **compiled-in only**. It
reuses the SDK's shared matcher (`sdk.MatchAll`).

## What it provides

| Capability | Surface |
|---|---|
| `verb:mount` | the `mount:` check/provision verb |

- **CHECK** — `findmnt` the mountpoint via the live check engine and match
  `source` / `filesystem` / `opt`.
- **ACT** — render an idempotent `findmnt || mount`.

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-mount/candy/plugin-mount:<tag>'
```

Then author the verb in a plan:

```yaml
- check: /proc is mounted as proc
  mount:
    mount: /proc
    filesystem: proc
  context: [runtime]
```

| Field | Meaning |
|---|---|
| `mount` | the mountpoint (also the scalar-sugar primary) |
| `mount_source` | expected source device |
| `filesystem` | expected filesystem type |
| `opt` | mount-option matchers — a matcher or list of matchers (`equals`, `not_equals`, `contains`, `not_contains`, `matches`, `not_matches`, `lt`, `le`, `gt`, `ge`) |

## Layout

- `candy/plugin-mount/` — the plugin module: `plugin.go` (provider + meta),
  `schema/mount.cue` (the self-contained `#MountInput`),
  `params/cue_types_gen.go`, and `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-internals:plugin` — the plugin/provider model,
  host-coupled verbs, and compiled-in placement (the candy carries no `skill:`
  entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291)).
- `/charly-check:check` — the declarative check-step surface the `mount:` verb is
  authored through.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
