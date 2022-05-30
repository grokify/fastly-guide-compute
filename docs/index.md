# Fastly Guide (Unofficial)

This is an unofficial companion guide to using Fastly Compute@Edge.

It builds on the official Fastly documentation with an additional focus on:

1. [Quick Start Guides to get a "Hello World" app working quickly](compute/quickstart_javascript/)
1. Day 2 usage to make everything as programmable as possible for [creating services](basics/service/), [packaging](compute/packaging/), [uploading and activation](compute/uploading_and_activation/).
1. Management Console usage for various operations including [creating services](basics/service/), [uploading packages, and activating services](compute/uploading_and_activation/).

## What is Compute@Edge?

Compute@Edge lets you deploy lightweight, fastly to run, serverless code at the edge, in the form of WebAssembly (WASM) packages. The WASM packages are standard definition files that use the WebAssembly System Interface (WASI).

See more on the [Fastly Product page](https://docs.fastly.com/products/compute-at-edge) and this guide's [FAQ](compute/faq).

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

> Note: Compute@Edge requires TLS for connections. Free TLS is provided for Fastly subdomains, but a paid account is required to use your own domain.

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

See the [Deployment page](compute/uploading_and_activation/) for more information on these approaches. The [Packaging page](compute/packaging/) covers the service package file format, along with approaches to create the service package.

Fastly also provides API Client SDKs in [Python](https://github.com/fastly/fastly-py), [Ruby](https://github.com/fastly/fastly-ruby), and [Perl](https://github.com/fastly/fastly-js). To request Upload Package API support, please post a GitHub Issue for their respective GitHub repositories.

Local testing can be done using [Viceroy](https://github.com/fastly/Viceroy).
