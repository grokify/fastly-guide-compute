# Using Viceroy and Alternative Hosts

## Viceroy

[Viceroy](https://github.com/fastly/Viceroy) is a local testing environment that mimics the Fastly Compute@Edge Wasm host to enable quick testing locally. It is built into the Fastly CLI and can be used on a stand-alone basis.

See the [Developer Guide](https://developer.fastly.com/learning/compute/testing/) for more information including constraints and limitations of the server.

> Note: While Fastly now uses the [Wasmtime](https://wasmtime.dev/) WebAssembly runtime, [Viceroy](https://github.com/fastly/Viceroy) still uses [Lucet](https://github.com/bytecodealliance/lucet). Bcause of this, there will be some minor differences in how they operate.

## Alternatives

* [fastlike](https://github.com/avidal/fastlike) by [Alex Vidal](https://github.com/avidal) provides an alternative host implementation using [wasmtime](https://github.com/bytecodealliance/wasmtime), which is the successor project to Lucet.
* [wazero](https://github.com/tetratelabs/wazero) is a zero dependency WebAssembly runtime in Go for which there is [interest to implement the Fastly ABI](https://github.com/tetratelabs/wazero/issues/662).