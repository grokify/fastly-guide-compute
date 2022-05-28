# The TOML Metadata file

The `fastly.toml` metadata file format is described in the [`fastly.toml` reference](https://developer.fastly.com/reference/compute/fastly-toml/)

This file does a number of things including:

1. metadata
1. build scripts
1. backends
1. dictionaries
1. logging endpoints

## Package Name

A basic `fastly.toml` file looks like the following:

```toml
manifest_version = 2
name = "my-compute-project"
description = "A wonderful Compute@Edge project that adds edge computing goodness to my application architecture."
authors = ["me@example.com"]
language = "rust"
service_id = "SU1Z0isxPaozGVKXdv0eY"
```