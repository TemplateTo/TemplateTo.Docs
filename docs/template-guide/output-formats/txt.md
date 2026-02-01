# Plain Text Generation

Render your templates as plain text (TXT) files. This is useful for email content, data exports, SMS messages, and any scenario where you need text without formatting.

## Overview

Plain text output is ideal for:

- Email body content (before HTML formatting)
- SMS or notification messages
- Data exports and reports
- Integration with systems that only accept plain text
- Generating configuration files or scripts

## How It Works

When you render a template as TXT:

1. All HTML elements are stripped
2. Text content is extracted in reading order
3. Variables are replaced with your data
4. The result is returned as a plain text file

## Template Considerations

### Keep It Simple

For best results with TXT output, use templates that focus on text content:

- Use **Text** elements for your content
- Avoid complex layouts (columns will be flattened)
- Tables will be converted to plain text (consider formatting)

### Variables Work the Same

All your variables and Liquid templating work identically:

```
Hello {{customerName}},

Your order #{{orderNumber}} has been shipped.

Items:
{% for item in items %}
- {{item.name}} (Qty: {{item.quantity}})
{% endfor %}

Total: {{total}}

Thank you for your business!
```

### Line Breaks

Use the Page Break element or multiple Text elements to control line spacing in your output.

## API Integration

### Generate TXT from Template

```bash
curl -X POST "https://api.templateto.com/render/txt/{templateId}" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"customerName": "Acme Corp", "orderNumber": "12345"}'
```

### Response

Returns the text file directly with `Content-Type: text/plain`.

### Async Rendering

For large documents or batch processing, use the async API:

```bash
curl -X POST "https://api.templateto.com/render/async/txt/{templateId}" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"customerName": "Acme Corp"}'
```

## Use Cases

### Email Templates

Create email body content that can be used with any email service:

```
Dear {{recipientName}},

{{emailBody}}

Best regards,
{{senderName}}
{{companyName}}
```

### Order Confirmations

Generate order summaries for SMS or simple notifications:

```
Order {{orderNumber}} confirmed!
Total: {{total}}
Delivery: {{deliveryDate}}
Track at: {{trackingUrl}}
```

### Data Reports

Export data in a readable text format:

```
Sales Report - {{reportDate}}
================================

Total Sales: {{totalSales}}
Orders: {{orderCount}}
Average Order: {{averageOrder}}

Top Products:
{% for product in topProducts %}
{{forloop.index}}. {{product.name}} - {{product.sales}}
{% endfor %}
```

## Comparison with Other Formats

| Feature | TXT | PDF | Image |
|---------|-----|-----|-------|
| File size | Smallest | Medium | Varies |
| Formatting | None | Full | Full |
| Universal compatibility | Highest | High | High |
| Email attachment friendly | Yes | Yes | Yes |
| Editable by recipient | Yes | Limited | No |

## Best Practices

1. **Design for text first** - If TXT is your primary output, keep templates simple
2. **Test the output** - Preview as TXT to see how formatting translates
3. **Use consistent spacing** - Control line breaks intentionally
4. **Consider character limits** - For SMS, keep content concise
5. **Avoid special characters** - Stick to ASCII for maximum compatibility

For complete API documentation, see the [REST API Reference](../../developer-guide/rest-api.md).
