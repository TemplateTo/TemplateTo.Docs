# Managing API Keys

API keys allow you integrate with TemplateTo from other services. That could be your own applications via our RestAPI or through Zapier/N8N. 

**Keep your keys safe**
Anyone can use your live mode secret API key to make any API call on behalf of your account, such as creating a charge or performing a refund. Keep your keys safe by following these best practices:

- Don’t store keys in a version control system.
- Control access to keys with a password manager or secrets management service.
- Don’t embed a key where it could be exposed to an attacker, such as in a mobile application.

## Create an API key

When creating an API key be sure to give it a meaningful name so that you are able to manage it in the future. Using the integration/application/workflow the key has been created for might work for you.

1. Open the [API keys](https://app.templateto.com/generate/api-keys) page
1. Click the Create key button
1. Give the new key a name, click save.
1. In the row for your new key, click Copy to obtain the generated key.

## Roll an API key

Rolling an API key removes the previous key from the system and generates a new, random key. 

!!! note
    Rolling an API key doesnt change the name in the dashboard, it updates the value of the key. All current integrations that make use of the key will fail until updated with the new value.

Roll a key in the following scenarios:

- If an API key is compromised, you need to remove or roll it to block any potentially malicious API requests that might use it.
- Your policy requires rotating keys at certain intervals.

Roll an API key in the UI:

1. Open the [API keys](https://app.templateto.com/generate/api-keys) page
1. In the row of the key you want to roll, click the Roll button.

Roll an API key via the API:

Coming soon

## Delete an API key

If you delete an API key, any code or integration that uses that key will no longer be able to make requests to our APIs. Be sure to create a new key and update any code/integrations to use it.

1. Open the [API keys](https://app.templateto.com/generate/api-keys) page
1. In the row of the key you want to delete, click the Delete button
1. In the confirmation dialogue click confirm.