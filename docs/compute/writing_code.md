# Writing WASM Edge Code using AssemblyScript

## The Code

### The Do-Nothing Function

The "do-nothing" function simply does what Fastly does by default by fetch and returning code from the back end. This is a useful way to start as other functionality will be modifications of this flow.

Dependencies:

```json
{
  "@fastly/as-compute": "^0.5.0"
}
```

Code:

```js
import { Fastly } from "@fastly/as-compute";

// Get the request from the client.
let req = Fastly.getClientRequest();

// Send the request to the backend server.
let beresp = Fastly.fetch(req, {
  backend: "origin_0",
  cacheOverride: null,
}).wait();

// Send the backend response back to the client.
Fastly.respondWith(beresp);
```

## Creating the WASM Binary

=== "JavaScript"
    **Creating the WASM binary using JavaScript**

    In your package's `package.json` file, ensure you define the `build` script. The following is an example from [github.com/fastly/compute-starter-kit-javascript-default](https://github.com/fastly/compute-starter-kit-javascript-default).

    ``` json
    {
      "scripts": {
        "prebuild": "webpack",
        "build": "js-compute-runtime --skip-pkg bin/index.js bin/main.wasm",
        "deploy": "npm run build && fastly compute deploy"
      }
    }
    ```

    To build the package using the Fastly CLI, run:

    ```
    % fastly compute build
    ```

    You can also run this manually by executing the `build` script via `npm`:

    ```
    % npm run build
    ```

=== "Swift"
    **Creating the WASM binary using Swift**

    Swift is enabled by the community Swift SDK for Fastly Compute: [`github.com/AndrewBarba/swift-compute-runtime`](https://github.com/AndrewBarba/swift-compute-runtime).

    The following is from an community example using this SDK: [`github.com/kateinoigakukun/swift-fastly-edge-example`](https://github.com/kateinoigakukun/swift-fastly-edge-example)

    `Package.swift`

    ```
    // swift-tools-version:5.5
    // The swift-tools-version declares the minimum version of Swift required to build this package.
    import PackageDescription

    let package = Package(
        name: "FastlyEdgeExample",
        dependencies: [
            .package(name: "Compute", url: "https://github.com/AndrewBarba/swift-compute-runtime", branch: "main")
        ],
        targets: [
            .executableTarget(name: "FastlyEdgeExample", dependencies: ["Compute"]),
        ]
    )
    ```

    Build the WASM binary:

    ``` bash
    % /Library/Developer/Toolchains/swift-wasm-5.6.0-RELEASE.xctoolchain/usr/bin/swift build --triple wasm32-unknown-wasi --product FastlyEdgeExample -c release
    ```

    This will build a binary in the following location:

    ```
    .build/release/FastlyEdgeExample.wasm
    ```


## Links

1. Examples: [https://developer.fastly.com/solutions/examples/assemblyscript/](https://developer.fastly.com/solutions/examples/assemblyscript/)
1. Fastly Fiddle: [https://fiddle.fastlydemo.net/](https://fiddle.fastlydemo.net/)