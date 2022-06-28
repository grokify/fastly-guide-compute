# Using Other Languages

Fastly's Compute@Edge supports WebAssembly System Interface (WASI) so langauges with this support can implement Fastly's Compute host calls to support using those languages for WebAssembly guests.

The information here buildes on the [Fastly Developer Guide section on Custom SDKs](https://developer.fastly.com/learning/compute/custom/).


## Fastly Compute@Edge ABI

The Fastly Application Binary Interface is documented in `.wtix` files which can be used to create language stubs to call Fastly's Compute@Edge's host calls. These are defined in the following files on Fastly's [Compute@Edge ABI definition](https://github.com/fastly/Viceroy/tree/main/lib/compute-at-edge-abi):

* [`README.md`](https://github.com/fastly/Viceroy/blob/main/lib/compute-at-edge-abi/README.md)
* [`compute-at-edge.witx`](https://github.com/fastly/Viceroy/blob/main/lib/compute-at-edge-abi/compute-at-edge.witx)
* [`typenames.witx`](https://github.com/fastly/Viceroy/blob/main/lib/compute-at-edge-abi/typenames.witx)

## Example Community SDKs

The following community SDKs are available as exmaple implementations when suppoorting new languages:

* [Swift Compute@Edge SDK](https://github.com/AndrewBarba/swift-compute-runtime) by [Andrew Barba](https://twitter.com/andrew_barba) ([host call definitions](https://github.com/AndrewBarba/swift-compute-runtime/blob/main/Sources/ComputeRuntime/include/ComputeRuntime.h))
* [Zig Compute@Edige SDK](https://github.com/jedisct1/zigly) by [Frank Denis](https://twitter.com/jedisct1) ([host call definitions](https://github.com/jedisct1/zigly/blob/master/src/zigly/wasm.zig))

## WITX-WebAssembly Code Generators

When implemeting a new language, scaffolding to create new server stubs can be done manually or can be done with a code generator. For code generates, the Rust-based [WITX-CodeGen](https://github.com/jedisct1/witx-codegen) by Frank Denis can be extended for more languages. It currently supports AssemblyScript, Zig and Rust, with interest to extend to C++, TinyGo and Swift.

## Language Candidates

To check if a language can be supported, check to see if it supports WebAssembly and WASI. A [list is maintained by Fermyon](https://www.fermyon.com/wasm-languages/webassembly-language-support) indicates the following additional languages have WASI support and are thus candicates for Fastly Compute@Edge guest language SDK support.

1. C
1. C++
1. C# and .NET
1. Go (via TinyGo WASI support)
1. Grain
1. Haskell
1. Motoko
1. Python
1. Ruby
