# Packaging the Edge Code

Before uploading to Fastly's Edge, the WASM binary, `fastly.toml` file and any additional files must be packaged into a gzipped tarball to be uploaded.

## Tarball Format

The file should be named `your-sanitized-package-name.tar.gz` and have the following files:

```
your-sanitized-package-name/bin/main.wasm
your-sanitized-package-name/fastly.toml
```

The sanitized package name is derived from the `name` property in the `fastly.toml` file. The algorithm to convert a string to one that is file system friendly is straight-forward and generally involves removing some characters like spaces and punctuation, along with removing diacritics.

For the Fastly CLI, the `name` property is run against the [`BaseName` function](https://pkg.go.dev/github.com/kennygrant/sanitize#BaseName) from the [`github.com/kennygrant/sanitize`](https://github.com/kennygrant/sanitize) package [as seen here in the CLI code](https://github.com/fastly/cli/blob/8e72711aac048f89f5bb9e576054757c1eae82f2/pkg/commands/compute/pack.go#L51).

## Packaging Examples

=== "CLI"
    **Creating the Edge Code package using the CLI**

    The [CLI is available from GitHub](https://github.com/fastly/cli) will automatically create the WASM binary and tarball file. Using the Fastly CLI is described in detail in the [Learning Guide](https://developer.fastly.com/learning/compute/).

    From the project root, execute the following command. The command for building the package file, `fastly compute init`, should have created the `fastly.toml` file in your project root.

    ``` bash
    % fastly compute build
    ```

    This will create the following files in the root directory:

    ```
    pkg/your-sanitized-package-name/bin/main.wasm
    pkg/your-sanitized-package-name/fastly.toml
    pkg/your-sanitized-package-name.tar.gz
    ```

    > Note: the files under the `pkg/your-sanitized-package-name` aren't needed. They are left-over artifacts from the packaging process to create the tarball.

=== "Go"
    **Creating the Edge Code package using the Go**

    Creating the tarball can be done using `archive/tar` in the standard library. A wapper in `github.com/grokify/fastlywasmly/tarutil` can be used to assist as so. This uses the Fastly CLI's Go code as a library to read the TOML file to create the package name.

    See more at [https://github.com/grokify/fastlywasmly](https://github.com/grokify/fastlywasmly)

    ``` go
    package main

    import (
        "fmt"
        
        "github.com/grokify/fastlywasmly/tarutil"
    )

    func main() {
        outfile, err := tarutil.BuildEdgePackage(
            "/path/to/fastly.toml",
            "/path/to/anything.wasm or empty if using bindir only",
            "/path/to/option/bin/dir or empty if using wasm only")
        if err != nil {
            panic(err)
        }
        fmt.Printf("WROTE [%s]\n", outfile)
    }
    ```  