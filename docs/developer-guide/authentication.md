# Authentication

All TemplateTo API endpoints require authentication using API keys. This guide covers how to create, manage, and securely use your API keys.

## Overview

API keys authenticate your requests to TemplateTo. Each key:

- Identifies your account
- Tracks usage and billing
- Can be revoked independently
- Should be kept secret

## Using API Keys

Pass your API key in the `X-Api-Key` header with every request:

```bash
curl -X POST "https://api.templateto.com/render/pdf/your-template-id" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"customerName": "Acme Corp"}'
```

### Example in Code

```javascript
const response = await fetch(
  `https://api.templateto.com/render/pdf/${templateId}`,
  {
    method: 'POST',
    headers: {
      'X-Api-Key': process.env.TEMPLATETO_API_KEY,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  }
);
```

## Managing API Keys

### Create an API Key

1. Open the [API Keys page](https://app.templateto.com/generate/api-keys)
2. Click **Create key**
3. Enter a descriptive name (e.g., "Production Server", "Zapier Integration")
4. Click **Save**
5. Copy the generated key immediately

!!! warning "Copy Your Key"
    The full key is only shown once. Copy it immediately and store it securely.

### Roll (Rotate) an API Key

Rolling a key generates a new value while keeping the same name. Use this for:

- Compromised keys that need replacement
- Regular key rotation per security policies
- Updating keys without changing your dashboard organization

**To roll a key:**

1. Open the [API Keys page](https://app.templateto.com/generate/api-keys)
2. Find the key to roll
3. Click the **Roll** button

!!! note
    Rolling a key invalidates the old value immediately. Update all integrations using that key before rolling.

### Delete an API Key

Delete keys you no longer need:

1. Open the [API Keys page](https://app.templateto.com/generate/api-keys)
2. Find the key to delete
3. Click the **Delete** button
4. Confirm deletion

!!! warning
    Deleting a key immediately breaks all integrations using it. Create a replacement key and update integrations first.

## Security Best Practices

### Keep Keys Secret

Anyone with your API key can make requests on your behalf. Protect your keys:

- **Never commit keys to version control** - Use environment variables instead
- **Don't expose keys in client-side code** - Keys should only be used server-side
- **Don't share keys via email or chat** - Use a secrets manager
- **Don't embed keys in mobile apps** - They can be extracted

### Use Environment Variables

Store keys in environment variables, not in code:

```javascript
// Good - key from environment
const apiKey = process.env.TEMPLATETO_API_KEY;

// Bad - hardcoded key
const apiKey = 'tt_live_abc123...'; // Don't do this!
```

### Separate Keys by Environment

Create different keys for different environments:

| Environment | Key Name | Purpose |
|-------------|----------|---------|
| Development | `Dev - Local Testing` | Testing during development |
| Staging | `Staging Server` | Pre-production testing |
| Production | `Production Server` | Live application |

### Rotate Keys Regularly

Establish a key rotation schedule:

- Roll keys periodically (e.g., quarterly)
- Roll immediately if compromise is suspected
- Remove unused keys promptly

### Use Secrets Management

For production systems, use a secrets manager:

- AWS Secrets Manager
- HashiCorp Vault
- Azure Key Vault
- Google Secret Manager

### Limit Access

Control who can access API keys:

- Only admins can create/delete keys in TemplateTo
- Editors can view but not manage keys
- Limit production key access to essential personnel

## Error Responses

### 401 Unauthorized

Returned when authentication fails:

```json
{
  "error": "Authentication required or invalid API key"
}
```

**Common causes:**

- Missing `X-Api-Key` header
- Invalid or malformed key
- Deleted or rolled key
- Typo in key value

### Troubleshooting

1. **Verify the header name** - Must be exactly `X-Api-Key`
2. **Check for extra spaces** - Keys should have no leading/trailing whitespace
3. **Confirm key status** - Check if key was rolled or deleted
4. **Test with a new key** - Create a fresh key to isolate the issue

## Integrations

### REST API

Use the `X-Api-Key` header directly:

```bash
curl -H "X-Api-Key: your-api-key" ...
```

### Zapier

Enter your API key when connecting your TemplateTo account in Zapier.

### N8N

Configure the TemplateTo node with your API key in the credentials section.

## Next Steps

- [REST API Reference](rest-api.md) - Full API documentation
- [Async Rendering](async-rendering.md) - Background document generation
- [Developer Guide Overview](index.md) - Integration options
