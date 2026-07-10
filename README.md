# rust-service-empty-archetype

Rust **Service Platform Overlay** — generates only the platform *servicing layer* for a
service and nothing else. Run it against an **existing** Rust project to retrofit it with:

- `.github/workflows/` — CI build + cut-tag (`rust-ci`)
- `.platform/kubernetes/**` — PlatformApplication CRD + dev/stg/prd kustomize overlays
  (`platform-application-manifests`), including `resourceRequirements` when you select a
  resource
- `.platform/docker/{local,prd}/Dockerfile`, `Tiltfile` — container builds +
  local-dev orchestration (idiomatic starting points; adapt to your build)
- `.editorconfig`, `.gitattributes`, `.gitignore`

It generates **no** project scaffolding and **no** domain code — no `Cargo.toml`, no
`crates/`, no `src/`, no `xtask/`, no `.justfile`. Everything renders **in place** at the
destination root.

```bash
archetect render git@github.com:p6m-archetypes/rust-service-empty-archetype.git#dev /path/to/existing-project
```

## Resources

You are prompted for persistence / cache / messaging so the PlatformApplication can
declare the matching `resourceRequirements`. **No connection code is generated** — this
overlay never touches project source. (Object storage is intentionally omitted: it drives
only code weaving, which does not apply to a retrofit.)

See `docs/specs/empty-service-archetype.md` in the archetype-ecosystem repo for the full
contract. This is the empty-tier sibling of `rust-rest-service-archetype` (full) and
`rust-service-basic-archetype` (minimal complete service).
