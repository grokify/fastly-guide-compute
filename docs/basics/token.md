# API Tokens Overview

API tokens are required for accessing the API directly and tools that use the API such as Fastly CLI.

The token is a alpha-numeric string that is supplied as the `Fastly-Key` HTTP request header in API requests.

## Creating Your Token

If you wish to manage tokens via the API, you will need to create your first token in the Console to be able to call the API to begin with.

Use the following steps:

1. Log into the Fastly Management Console at https://manage.fastly.com
2. Click your name in the upper left corer to open the menu and then click on "Account"
3. In the left nav sidebar click on the menu item for "Personal API tokens" under "Your profile"
4. Click "+ Create Token"

## Learn More

You can learn about these tokens here:

1. [Managing tokens via the Console](https://docs.fastly.com/en/guides/using-api-tokens)
1. [Managing tokens via the API](https://developer.fastly.com/reference/api/auth/)