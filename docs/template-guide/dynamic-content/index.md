# Dynamic Content

Make your templates data-driven with variables, Liquid templating, and shared themes.

## In This Section

<div class="grid cards" markdown>

- :material-variable:{ .lg .middle } **Variables & Data**

    ---

    Use placeholders, JSON data, and Liquid filters

    [:octicons-arrow-right-24: Variables](variables.md)

- :material-palette:{ .lg .middle } **Themes**

    ---

    Share CSS styling across multiple templates

    [:octicons-arrow-right-24: Themes](themes.md)

</div>

## Overview

Dynamic content allows you to create reusable templates that generate unique documents for each render. Instead of hardcoding values, you use variables that get replaced with real data at render time.

### How It Works

1. **Add placeholders** in your template using `{{variableName}}` syntax
2. **Send data** as JSON when calling the API
3. **Receive personalized documents** with your data inserted

### Example

**Template:**
```
Invoice for {{customerName}}
Amount due: {{total}}
```

**API Data:**
```json
{
  "customerName": "Acme Corp",
  "total": "$1,500.00"
}
```

**Output:**
```
Invoice for Acme Corp
Amount due: $1,500.00
```

## Quick Links

- [Liquid Documentation](https://shopify.github.io/liquid/) - Official Liquid templating guide
- [Elements Reference](../creating/elements.md) - Using variables in different elements
- [REST API](../../developer-guide/rest-api.md) - Sending data via API
