# Using the engine from a host project

The engine has no dependency on any host. It started life as the `quagmire-engine/` folder of
the [Testing Reality](https://github.com/spinsphere/testing-reality) repository and was split
out with `git subtree split` (the commit history of that folder is this repository's history).

## Layout

`Cargo.toml` (workspace), `.cargo/config.toml` (wasm and Windows link flags), `core/`
(`quagmire-core`), `sdk-godot/` (`quagmire-godot`, including `build.sh`, the per-target
build), `docs/`, `rust-toolchain.toml`.

## Consuming it

- **Godot hosts** add this repository as a git submodule and run
  `<engine>/sdk-godot/build.sh <linux|windows|web> [release|debug]`, which prints the path of
  the built library. The host copies that file wherever its `.gdextension` expects it
  (Testing Reality: `tools/build-sdk.sh`, which commits the binaries under
  `app/addons/quagmire/bin/` so a clone opens in Godot without a Rust toolchain).
- The GDScript API (`QuagmireSystem`, `QuagmireScreen`, `QuagmireProfile`) is documented in
  the doc comments of `sdk-godot/src/lib.rs`.
- **Other engines**: a C ABI crate (`sdk-c`, opaque handles, caller-allocated buffers) for
  Unity / Unreal / C++ is the next planned addition.

## Conformance suite

`core/tests/golden.rs` encodes the numbers derived on the Testing Reality site and is the
engine's conformance suite; it does not depend on any host.
