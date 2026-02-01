# Developer Guide

Integrate TemplateTo into your application to automate PDF and image generation.

## Quick Links

<div class="grid cards" markdown>

- :material-key:{ .lg .middle } **Authentication**

    ---

    Set up API keys and secure your integration

    [:octicons-arrow-right-24: Authentication](authentication.md)

- :material-api:{ .lg .middle } **REST API Reference**

    ---

    Complete API documentation with examples

    [:octicons-arrow-right-24: REST API](rest-api.md)

- :material-clock-fast:{ .lg .middle } **Async Rendering**

    ---

    Background processing for large documents

    [:octicons-arrow-right-24: Async API](async-rendering.md)

- :material-puzzle:{ .lg .middle } **No-Code Integrations**

    ---

    Connect via Zapier or N8N without coding

    [:octicons-arrow-right-24: No-Code](no-code/zapier.md)

</div>

## Integration Paths

### Direct API Integration

Use the REST API for full control over document generation.

**Best for:**

- Custom application integrations
- High-volume document generation
- Programmatic workflows
- Developers comfortable with HTTP APIs

**Features:**

- Synchronous and asynchronous rendering
- PDF, PNG, JPEG, and TXT output
- Template-based or raw HTML input
- Webhooks for async completion notifications

### No-Code Platforms

Connect TemplateTo with Zapier or N8N for automation without writing code.

**Best for:**

- Workflow automation
- Connecting existing tools
- Non-technical users
- Quick integrations

**Supported platforms:**

- [Zapier](no-code/zapier.md) - Connect with 5,000+ apps
- [N8N](no-code/n8n.md) - Self-hosted automation platform

## Getting Started

### 1. Create an API Key

Generate an API key in your account settings:

1. Go to [API Keys page](https://app.templateto.com/generate/api-keys)
2. Click **Create New API Key**
3. Give it a descriptive name
4. Copy and securely store the key

See [Authentication](authentication.md) for security best practices.

### 2. Choose Your Integration Method

| Method | Use Case |
|--------|----------|
| [REST API](rest-api.md) | Custom integrations, full control |
| [Async API](async-rendering.md) | Large documents, background processing |
| [Zapier](no-code/zapier.md) | No-code automation |
| [N8N](no-code/n8n.md) | Self-hosted automation |

### 3. Make Your First Request

```bash
curl -X POST "https://api.templateto.com/render/pdf/your-template-id" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"customerName": "Acme Corp", "total": 150.00}'
```

## Code Builder

Generate code snippets in multiple languages based on your template:

[:octicons-arrow-right-24: Open Code Builder](https://app.templateto.com/generate/code-builder)

The code builder creates ready-to-use code for:

- JavaScript/Node.js
- Python
- C#
- cURL
- And more

## Base URL

All API endpoints use:

```
https://api.templateto.com
```

## Output Formats

| Format | Content Type | Use Case |
|--------|--------------|----------|
| PDF | `application/pdf` | Documents, reports, invoices |
| PNG | `image/png` | Graphics, images with transparency |
| JPEG | `image/jpeg` | Photographs, smaller file sizes |
| TXT | `text/plain` | Plain text extraction |

## Need Help?

- **API Reference** - [REST API documentation](rest-api.md)
- **Code Examples** - [Code Builder](https://app.templateto.com/generate/code-builder)
- **Template Setup** - [Template Builder Guide](../template-guide/editor-overview.md)
