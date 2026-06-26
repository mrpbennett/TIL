# Building Rust Projects with Embedded WASM Plugins

When a Rust binary embeds compiled WebAssembly plugins via `include_bytes!`, the
WASM files must be compiled **separately** before the main binary — Cargo does not
do this automatically.

## How embedding works

The host binary uses `include_bytes!` at compile time to bake the WASM directly
into the binary:

```rust
// zellij-utils/src/consts.rs
include_bytes!(concat!(
    env!("CARGO_MANIFEST_DIR"),
    "/assets/plugins/session-manager.wasm"
))
```

This means if you edit plugin source and rebuild the binary without rebuilding
the WASM first, the binary silently runs **stale plugin code** — no error, no
warning.

## The `plugins_from_target` feature

A common pattern is a Cargo feature that switches the embed path in debug builds:

```rust
#[cfg(any(not(feature = "plugins_from_target"), not(debug_assertions)))]
include_bytes!("assets/plugins/session-manager.wasm")  // release: pre-built asset

#[cfg(all(feature = "plugins_from_target", debug_assertions))]
include_bytes!("../target/wasm32-wasip1/debug/session-manager.wasm")  // debug: from target/
```

`plugins_from_target` is enabled by default. This means:

- **Debug builds** read WASM from `target/wasm32-wasip1/debug/` — you still need
  to build the WASM, but you skip the copy step.
- **Release builds** always read from `assets/plugins/` — you must copy manually.

## Build workflow

### Release

```bash
# 1. Compile the plugin to WASM
cargo build -p session-manager --target wasm32-wasip1 --release

# 2. Copy into assets (embedded at compile time)
cp target/wasm32-wasip1/release/session-manager.wasm zellij-utils/assets/plugins/

# 3. Build the host binary
cargo build --release
```

### Debug (faster iteration)

```bash
# 1. Compile the plugin to WASM (debug target)
cargo build -p session-manager --target wasm32-wasip1

# 2. Build the host — picks up WASM from target/ automatically
cargo build

./target/debug/zellij
```

## Key takeaway

Cargo workspaces don't cross compile targets automatically. A plugin crate
targeting `wasm32-wasip1` and a host binary targeting the native target are
treated as completely independent build graphs — you have to drive each one
explicitly.
