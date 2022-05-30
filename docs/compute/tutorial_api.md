# Uploading and Activation via the API

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

## Create a New Service

Use the following steps to create New Service entirely using APIs.

1. Create the new service with the ["Create a service" API](https://developer.fastly.com/reference/api/services/service/#create-service).
1. Add one or more Domains by calling the ["Add a domain name to a service" API](https://developer.fastly.com/reference/api/services/domain/#create-domain).
1. Add one or more backend hosts by calling the ["Create a backend" API](https://developer.fastly.com/reference/api/services/backend/#create-backend).
1. To upload the Edge code package tarball, call the ["Upload a Compute@Edge package" API](https://developer.fastly.com/reference/api/services/package/#put-package).
1. Activate the new Edge code by calling the [Activate a service version](https://developer.fastly.com/reference/api/services/version/#activate-service-version).

Using cURL, the commands look like the following:

["Create a service" API](https://developer.fastly.com/reference/api/services/service/#create-service). For the next steps, use the service version that you receive back in the API response. This in an auto-incrementing integer, e.g. `1`, `2`, etc. See more in the ["Creating a Service" section](../../basics/service).

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

["Activate a service version" API](https://developer.fastly.com/reference/api/services/version/#activate-service-version):

```
% curl -D - -X PUT --location "https://api.fastly.com/service/{YOUR_SERVICE_ID}/version/{YOUR_SERVICE_VERSION}/activate" \
  -H "Accept: application/json" \
  -H "Fastly-Key: YOUR_FASTLY_TOKEN"
```

## Update an Existing Service

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