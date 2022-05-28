# Creating a Service

A Compute@Edge service must be created for each serverless function you want to run. Each service can listen to one or more domains, but each domain can only map to a single service.

A Compute@Edge service can be created by the CLI, UI, API or API SDKs.

> Note: after you have a service, be sure to [add a domain so it can be accessed](../dns_and_tls).

The CLI and SDKs both use the ["Create a service" API](https://developer.fastly.com/reference/api/services/service/#create-service). Note that this API requires the `engineer` role. The `superuser` role does not satisfy the requirements for this API.

## Using the Console

In the console, click on the "Compute" menu item:

![](fastly_console_menu_compute.png)

Click on the "Create a Compute service" button:

![](fastly_console_button_create-a-compute-service.png) 

Note the Service ID beside your service name on the resulting page"

![](fastly_compute_console_service-name-and-id.png)

## Using the CLI and API

> Note: to use the CLI and API/SDKs, the token must be issued for a user with the `engineer` role. See the [Troubleshooting section below](#using-the-superuser-permission) for error messages you will encounter using the `superuser` role.

For additional SDK examples, see [the API Reference for the "Create a service" API](https://developer.fastly.com/reference/api/services/service/#create-service).

=== "CLI"

    ```bash
    % fastly service create --name="your-service-name" --type="wasm"

    SUCCESS: Created service 2KX9IIEH4ppKNsa6MZBwPJ
    ```

=== "cURL"

    ``` bash
    % curl -D - -X POST --location "https://api.fastly.com/service" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -H "Accept: application/json" \
    -H "Fastly-Key: YOUR_FASTLY_TOKEN" \
    -d "name=your-service-name&type=wasm"
    ```

=== "HTTP"

    ``` bash
    POST /service HTTP/1.1
    Host: api.fastly.com
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    Fastly-Key: YOUR_FASTLY_TOKEN
    
    name=your-service-name&type=wasm
    ```

## Troubleshooting

### Using the Superuser Permission

While a user with the `superuser` role can create a service in the Fastly Management Console, the same role cannot be used to create a service via the API. The `engineer` role is required.

If you attempt to create a service with a `superuser` role you will receive the following errors:

#### Error Using the CLI

Attempting to create a service using the Fastly CLI with a `superuser` role will result in the following error:

```bash
ERROR: the Fastly API returned 404 Not Found: \
Record not found (Cannot find 'service').
```

#### Error Using the API

Attempting to create a service using the Fastly API with a `superuser` role will result in the following error:

```bash
{"msg":"Record not found","detail":"Cannot find 'service'"}
```

#### Resolution

To resolve this issue, use an API Token for a user with the `engineer` access control role.

#### Workarounds

A workaround is to use the Fastly Management Console to create services.
