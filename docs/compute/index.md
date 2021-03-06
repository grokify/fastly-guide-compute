# Compute@Edge

Compute@Edge lets you deploy serverless code at the edge, in the form of WebAssembly (WASM) packages. The WASM packages are standard definition files that use the WebAssembly System Interface (WASI).

## Deployment Process

There are generally 5 steps for deploying code at Fastly's Edge, some of which can be combined and reordered by some tools such as the [Fastly CLI](https://github.com/fastly/cli).

1. Create the WASM binary
1. Create the `fastly.toml` file
1. Create the Edge Code package tarball
1. Upload the Edge Code package tarball
1. Activate the service

Prerequisites include:

1. Create an account
1. Create a Compute@Edge service
1. Add a domain
1. Add TLS certificate, which is required for Compute@Edge

> Note: Compute@Edge requires TLS for connections, which requires a paid account.

## Edge Code Language Support

Code can target WebAssembly in a variety of languages. The following are available with support from Fastly and the community.

1. JavaScript (Fastly): [reference](https://js-compute-reference-docs.edgecompute.app/), [NPM](https://www.npmjs.com/package/@fastly/js-compute)
1. AssemblyScript (Fastly): [reference](https://as-compute-reference-docs.edgecompute.app/), [NPM](https://www.npmjs.com/package/@fastly/as-compute)
1. Rust (Fastly): [reference](https://docs.rs/fastly/latest/fastly/)
1. Swift (community): [repo](https://github.com/AndrewBarba/swift-compute-runtime)
1. Zig (community): [repo](https://github.com/jedisct1/zigly)

Fastly's support team offers support on Fastly SDKs, while community projects are supported by their author's and the community. See the [community SDKs page](https://developer.fastly.com/learning/compute/custom/) for general info on how to use these.

For a discussion on new language support, including Go, check out the article on [Evaluating new languages for Compute@Edge](https://www.fastly.com/blog/evaluating-new-languages-for-edge-compute).

## Deployment Options

Fastly offers several ways to deploy Fastly edge code packages to production.

1. [Fastly CLI](https://developer.fastly.com/learning/compute/) - an easy way to create and upload packages manually
1. [Fastly Management Console](https://manage.fastly.com/) - upload packages via drag-and-drop from your filesystem
1. [Fastly API](https://developer.fastly.com/reference/api/) - upload packages via [Fastly's "Upload a Compute@Edge package" API](https://developer.fastly.com/reference/api/services/package/#put-package)
1. [Fastly API Client SDK for Go](https://github.com/fastly/go-fastly) - upload packages via the [`UpdatePackage` function](https://pkg.go.dev/github.com/fastly/go-fastly/v6/fastly#Client.UpdatePackage)
1. [Fastly API Client SDK for JavaScript](https://github.com/fastly/fastly-js) - upload packages via the [`putPackage` function](https://github.com/fastly/fastly-js/blob/main/docs/PackageApi.md#putPackage)
1. [Fastly API Client SDK for PHP](https://github.com/fastly/fastly-php) - upload packages via the [`putPackage` function](https://github.com/fastly/fastly-php/blob/main/docs/Api/PackageApi.md#putpackage)

Fastly also provides API Client SDKs in [Python](https://github.com/fastly/fastly-py), [Ruby](https://github.com/fastly/fastly-ruby), and [Perl](https://github.com/fastly/fastly-js). To request Upload Package API support, please post a GitHub Issue for their respective GitHub repositories.

Local testing can be done using [Viceroy](https://github.com/fastly/Viceroy).

## Frequently Asked Questions

### How does WebAssembly compare with Docker?

WebAssembly has some advantages over Docker including:

![](wasmvdocker.png)

See more in this article by [Will Cloud-Native WebAssembly Replace Docker? by Michael Yuan and Yicen Xie](
https://kubesphere.io/blogs/will-cloud-native-webassembly-replace-docker_/)

Docker has some advantages over WebAssembly, including the ability to support more languages.