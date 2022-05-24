# Tokio-echo-test

## Compilation:
1. `rustup target add wasm32-wasi`
2. `cargo build --target=wasm32-wasi`

## Testing:
WebAssembly cannot execute `socket()`, `bind()`, or `listen()`. So instead, the WebAssembly runtime must open the socket and pass the file descriptor to the module. From there, WebAssembly may `read()`, `write()`, and handle connections.

## Running with Enarx:
1. Install Enarx from https://github.com/enarx/enarx.
2. Compile with `cargo build`, optionally with `cargo build --features "dbg"` to see a lot of additional debugging output.
3. Use the provided Enarx.toml file: `enarx run --wasmcfgfile Enarx.toml target/wasm32-wasi/debug/tokio-echo-test.wasm`.

## Running with Wasmtime
1. Download and install Wasmtime from https://github.com/bytecodealliance/wasmtime
   * Optionally install Wasmtime with this script: `curl https://wasmtime.dev/install.sh -sSf | bash`
2. Provide the socket file descriptor information on the command line: `wasmtime run --tcplisten 127.0.0.1:8080 --env FD_COUNT=3 target/wasm32-wasi/debug/tokio-echo-test.wasm `.

## Connecting
Use Netcat to connect after "Listening" is displayed, `nc localhost 8080`.