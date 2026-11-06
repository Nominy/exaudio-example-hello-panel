# Hello panel — Exaudio example extension

`example.hello-panel` is the reference WASM extension for [Exaudio](https://github.com/Nominy/exaudio).
It contributes a **Hello panel** command panel that inserts a greeting into the
active document, publishes a diagnostic for empty segments, and registers a
synthetic `Hello account` provider plus a `Hello notes` mode: "signing in" stores
a display name in the instance secret store, and the mode opens a notes document
greeting that name. It is entirely offline: no network, filesystem, clock or
random source is available to a component.

These sources are the same files as `extensions/examples/hello-panel` in the
Exaudio repository. The `exaudio-sdk` dependency in `Cargo.toml` is a path
dependency on `sdk/rust` of that repository, so the crate builds inside an
Exaudio checkout (clone `Nominy/exaudio` and build at
`extensions/examples/hello-panel`), or after pointing the dependency at
`git = "https://github.com/Nominy/exaudio"` in a standalone clone.

## Build and package

Requires the `wasm32-unknown-unknown` Rust target and `wasm-tools`.

```powershell
cargo build --target wasm32-unknown-unknown --release
wasm-tools component new target/wasm32-unknown-unknown/release/exaudio_example_hello_panel.wasm -o module.wasm
pwsh -File package.ps1 -Component module.wasm
```

`package.ps1` writes an immutable `dist/example.hello-panel-<version>.exaudio`
(ZIP with `manifest.json`, `module.wasm`, `assets/LICENSE`) and prints its byte
size and SHA-256. It refuses to overwrite an existing artifact. Do not publish a
`runtime-probes` feature build; that feature only exists for the host smoke test
(`smoke.ps1`, which runs inside the Exaudio workspace).

## Releases and the index

Each version is published as a GitHub release of this repository whose single
asset is the `.exaudio` package. The reviewed catalog
[Nominy/exaudio-extensions](https://github.com/Nominy/exaudio-extensions) records
the release tag, source commit, manifest, asset URL, byte size and SHA-256; the
app installs only bytes that match that record.

| Version | Tag | Asset |
| --- | --- | --- |
| 0.1.0 | `v0.1.0` | `example.hello-panel-0.1.0.exaudio` |

## Permissions

The manifest declares `document-read`, `document-edit` and `secrets`. Declaring a
capability requests it; the user grants it at install time.

## License

MIT — see [LICENSE](LICENSE).
