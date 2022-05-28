# Getting Started with Compute@Edge using the API

This is an alternative guide to [Fastly's official guide](https://developer.fastly.com/learning/compute/) which focuses API-based deployment without any client software. The official guide uses the [Fastly CLI](https://github.com/fastly/cli) which has the benefit of automating many of the tasks required from the command line. This is good for testing, however, for deployment at scale via DevOps, it is nice to be able to programmatically automate these tasks.

This guide uses no specific client tools so that the process can be understood more in-depth and automated with code in any language of your choice. The Fastly CLI uses Go and the [Fastly Go SDK](https://github.com/fastly/go-fastly). While the author of this document uses Go and could have written a guide to automate the Compute@Edge process using Go package functions, a language-independent approach was selected for this guide for broader applicability. In the future, a programmable Go guide may be provided as well.

## Steps

This guide will create and deploy new Edge Code using the following steps:

1. Create the WASM binary
1. Create the Edge Package `fastly.toml` configuration file
1. Create the Edge Package Archive Gzipped tarball archive file
1. Upload and activate the package

## Prerequisites

1. [Have a Fastly Account](../../overview/account)
2. [Have a Fastly API Token](../../overview/token)
3. Have a host domain and backend host/IP address with the DNS server for the host domain pointing to Fastly. See more in [Adding CNAME records](https://docs.fastly.com/en/guides/adding-cname-records)

## Step 1: Create the WASM binary

Fastly is designed to accept any WASI-compatible WASM binary, so there's no need for any special client tools to do this. For this tutorial, the following Swift example has been verified to work. In the future, examples for other languages including JavaScript, AssemblyScript and Rust may be provided.

[https://github.com/kateinoigakukun/swift-fastly-edge-example](https://github.com/kateinoigakukun/swift-fastly-edge-example)

In this unofficial example, the WASM binary can be built with the following command:

`/Library/Developer/Toolchains/swift-wasm-5.6.0-RELEASE.xctoolchain/usr/bin/swift build --triple wasm32-unknown-wasi --product FastlyEdgeExample -c release`

This will produce the following file:

`./.build/release/FastlyEdgeExample.wasm`

## Step 2: Create the TOML configuration file

A configuraiton file is a TOML text file that looks like the following. The structure of this file is described in the [fastly.toml package manifest format](https://developer.fastly.com/reference/compute/fastly-toml/) reference page.

Of note, the `service_id` does not have to be populated in this manifest file as it can be provided in the Upload Package API call later. Leaving this empty will allow the `fastly.toml` file to be used across multiple Edge services.

```
# This file describes a Fastly Compute@Edge package. To learn more visit:
# https://developer.fastly.com/reference/fastly-toml/

authors = ["kateinoigakukun"]
description = "ping-pong"
language = "other"
manifest_version = 2
name = "swift-fastly-edge-example"
service_id = ""
```

Using Fastly CLI, this file is created by the `fastly compute init` command.

## Step 3: Create the Edge Package file

The Edge Package file is a Gzipped Tarball which is uploaded to Fastly using the Service Update Package API.

The file has the following contents:

```
bin/main.wasm
fastly.toml
```

* The `main.wasm` file is the same file as created in Step 1.
* The `fastly.toml` file is the same file as created in Step 2.

Using Fastly CLI, this file is created by the `fastly compute build` command. The file name created by the Fastly CLI uses the format `my-project.tar.gz` though it needs to be confirmed if the file name matters. Fastly CLI will create the following directories and files in your root directory:

```
pkg/my-project/bin/main.wasm
pkg/my-project/fastly.toml
pkg/my-project.tar.gz
```

From this, the `my-project.tar.gz` file is needed for uploading to Fastly.

## Step 4 Upload the Package

This guide covers 2 ways to upload the package:

1. From scratch with no pre-existing service
2. With a pre-existing service

Both of these apporaches use the API for complete automation support.

Both of these approaches have parts that can be performed with the UI includign:

1. Creating a service
2. Adding domains to a service
3. Adding backends to a service

Using Fastly CLI, deployment and activiation is accomplished by the `fastly compute deploy` command.

Uploading the Package is performed via the REST API, which is where the serviceID and API token are needed.

## Step 4.1 Create a New Service

Use the following steps to create New Service entirely using APIs.

1. Create the new service with the ["Create a service" API](https://developer.fastly.com/reference/api/services/service/#create-service).
1. Add one or more Domains by calling the ["Add a domain name to a service" API](https://developer.fastly.com/reference/api/services/domain/#create-domain).
1. Add one or more backend hosts by calling the ["Create a backend" API](https://developer.fastly.com/reference/api/services/backend/#create-backend).
1. To upload the Edge code package tarball, call the ["Upload a Compute@Edge package" API](https://developer.fastly.com/reference/api/services/package/#put-package).
1. Activate the new Edge code by calling the [Activate a service version](https://developer.fastly.com/reference/api/services/version/#activate-service-version).

Using cURL, the commands look like the following:

["Create a service" API](https://developer.fastly.com/reference/api/services/service/#create-service). For the next steps, use the service version that you receive back in the API response. This in an auto-incrementing integer, e.g. `1`, `2`, etc.

```
% curl -D - -X POST --location "https://api.fastly.com/service" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -H "Fastly-Key: {YOUR_FASTLY_TOKEN}" \
  -d "name=test-service"
```

["Add a domain name to a service" API](https://developer.fastly.com/reference/api/services/domain/#create-domain):

```
% curl -D - -X POST --location "https://api.fastly.com/service/{YOUR_SERVICE_ID}/version/{YOUR_SERVICE_VERSION}/domain" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -H "Fastly-Key: {YOUR_FASTLY_TOKEN}" \
  -d "name=www.example.com"
```

["Create a backend" API](https://developer.fastly.com/reference/api/services/backend/#create-backend):

```
% curl -D - -X POST --location "https://api.fastly.com/service/{YOUR_SERVICE_ID}/version/{YOUR_SERVICE_VERSION}/backend" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Accept: application/json" \
  -H "Fastly-Key: YOUR_FASTLY_TOKEN" \
  -d "address=127.0.0.1&name=test-backend&port=80"
```

["Upload a Compute@Edge package" API](https://developer.fastly.com/reference/api/services/package/#put-package):

```
% curl -D - -X PUT --location "https://api.fastly.com/service/{YOUR_SERVICE_ID}/version/{YOUR_SERVICE_VERSION}/package" \
  -H "Accept: application/json" \
  -H "Fastly-Key: YOUR_FASTLY_TOKEN" \
  -H "expect: 100-continue" \
  -F "package=@/path/to/file"
```

[Activate a service version](https://developer.fastly.com/reference/api/services/version/#activate-service-version):

```
% curl -D - -X PUT --location "https://api.fastly.com/service/{YOUR_SERVICE_ID}/version/{YOUR_SERVICE_VERSION}/activate" \
  -H "Accept: application/json" \
  -H "Fastly-Key: YOUR_FASTLY_TOKEN"
```

## Step 4.2: Update an Existing Service

Cloning an existing service will carry over existing configuration including the domains and backend hosts. By cloning a service where these are configured in the current configuration, the only additional steps are to (a) upload the package and (b) activate the service version.

1. Create the new service with the ["Clone a service version" API](https://developer.fastly.com/reference/api/services/version/#clone-service-version)
1. To upload the Edge code package tarball, call the ["Upload a Compute@Edge package" API](https://developer.fastly.com/reference/api/services/package/#put-package).
1. Activate the new Edge code by calling the [Activate a service version](https://developer.fastly.com/reference/api/services/version/#activate-service-version).

["Clone a service version" API](https://developer.fastly.com/reference/api/services/version/#clone-service-version):

```
% curl -D - -X PUT --location "https://api.fastly.com/service/SU1Z0isxPaozGVKXdv0eY/version/1/clone" \
  -H "Accept: application/json" \
  -H "Fastly-Key: YOUR_FASTLY_TOKEN"
```

Steps for uploading a package and activating a service version are as shown above.