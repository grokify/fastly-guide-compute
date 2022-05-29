# Quickstart for WASM Edge Code using Swift

This Quickstart uses the [`github.com/kateinoigakukun/swift-fastly-edge-example`](https://github.com/kateinoigakukun/swift-fastly-edge-example) example, which uses the [`github.com/AndrewBarba/swift-compute-runtime`](https://github.com/AndrewBarba/swift-compute-runtime) SDK.

## Prerequisites

1. Install the Swift WASM toolchain from the links at [SwiftWASM.org](https://book.swiftwasm.org/getting-started/setup.html)
1. Install the [Fastly CLI](https://github.com/fastly/cli)

## Quickstart using the CLI:

```
$ /Library/Developer/Toolchains/swift-wasm-5.5.0-RELEASE.xctoolchain/usr/bin/swift build --triple wasm32-unknown-wasi --product FastlyEdgeExample -c release
$ fastly compute init
$ fastly compute pack --wasm-binary=./.build/release/FastlyEdgeExample.wasm
$ fastly compute deploy
```

## Additional Approachs

See the following pages for using alternative methods for packaging, uploading and activation.

1. [Packaging](../packaging) includes information on file structure for creation using Go and manually
1. [Uploading & Activating](../uploading_and_activation) includes information on uploading using the CLI, Fastly Management Console, API, Go SDk, JS SDK and PHP SDK.