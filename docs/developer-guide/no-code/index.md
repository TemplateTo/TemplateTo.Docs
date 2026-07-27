# No-Code Integrations

Connect TemplateTo with automation platforms to generate documents without writing code.

## Available Integrations

<div class="grid cards" markdown>

- :simple-zapier:{ .lg .middle } **Zapier**

    ---

    Connect with 5,000+ apps to automate document generation

    [:octicons-arrow-right-24: Zapier Guide](zapier.md)

- :material-vector-polyline:{ .lg .middle } **N8N**

    ---

    Self-hosted automation with advanced workflow capabilities

    [:octicons-arrow-right-24: N8N Guide](n8n.md)

- :material-factory:{ .lg .middle } **Odoo**

    ---

    Replace wkhtmltopdf with Chromium-based PDF rendering in Odoo 18 and 19

    [:octicons-arrow-right-24: Odoo Guide](odoo.md)

</div>

## When to Use No-Code

No-code integrations are ideal when you want to:

- **Automate without developers** - Set up document generation without coding
- **Connect existing tools** - Link TemplateTo with your CRM, forms, or spreadsheets
- **Build workflows quickly** - Create automations in minutes, not days
- **React to events** - Generate documents when triggers occur (new order, form submission, etc.)

## Common Use Cases

### With Zapier

- Generate invoices when new orders arrive in Shopify
- Create contracts from Google Forms submissions
- Send personalized PDFs via Gmail
- Save generated documents to Google Drive or Dropbox

### With N8N

- Build complex multi-step workflows
- Self-host for data privacy requirements
- Advanced data transformation before rendering
- Error handling and conditional logic

### With Odoo

- Replace wkhtmltopdf globally with a single module install
- Generate month-end invoices in bulk with batch processing
- Get modern CSS in QWeb report templates (flexbox, grid, web fonts)
- Automatic fallback to wkhtmltopdf if the API is unreachable

## Comparison

| Feature | Zapier | N8N | Odoo |
|---------|--------|-----|------|
| Setup complexity | Easy | Moderate | Easy |
| Integration type | Workflow automation | Workflow automation | PDF engine replacement |
| Hosting | Cloud only | Cloud or self-hosted | Self-hosted (Odoo addon) |
| Pricing | Per-task | Free (self-hosted) | Free addon + paid TemplateTo service |
| Data transformation | Basic | Advanced | N/A (uses QWeb templates) |
| Custom code | Limited | Full support | N/A (drop-in replacement) |

## Getting Started

1. **Review [TemplateTo pricing](https://templateto.com/#pricing)** and select a paid plan
2. **Create an API key** in your [TemplateTo account](https://app.templateto.com/generate/api-keys)
3. **Choose your platform** - Zapier for simplicity, N8N for flexibility, Odoo for PDF engine replacement
4. **Follow the setup guide** for your chosen platform
5. **Build your first automation** connecting a trigger to TemplateTo

## Need More Control?

If no-code platforms don't meet your needs, consider:

- [REST API](../rest-api.md) - Full programmatic control
- [Async Rendering](../async-rendering.md) - Background processing for large documents
