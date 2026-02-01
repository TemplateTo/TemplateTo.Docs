# Localization

TemplateTo supports culture-aware formatting for numbers, currencies, and dates. Set the culture to control how values are displayed for different regions.

## Setting Culture

Culture is set per-render via the API or template settings. The culture name follows the standard format: `language-REGION` (e.g., `en-US`, `en-GB`, `de-DE`).

### Via API

Include `cultureName` in your render request:

```json
{
  "templateId": "tpl_abc123",
  "data": { ... },
  "cultureName": "en-GB"
}
```

### Via Template Settings

Set a default culture in your template's settings for consistent formatting.

## Culture Effects

### Currency Formatting

The `format_number: "C"` filter uses the culture's currency symbol and format:

| Culture | `{{ 1234.50 \| format_number: "C" }}` |
|---------|----------------------------------------|
| `en-US` | $1,234.50 |
| `en-GB` | £1,234.50 |
| `de-DE` | 1.234,50 € |
| `fr-FR` | 1 234,50 € |
| `ja-JP` | ¥1,235 |

### Number Formatting

Thousand separators and decimal points vary by culture:

| Culture | `{{ 1234567.89 \| format_number: "N2" }}` |
|---------|-------------------------------------------|
| `en-US` | 1,234,567.89 |
| `de-DE` | 1.234.567,89 |
| `fr-FR` | 1 234 567,89 |

### Date Formatting

Date formats respect cultural conventions:

| Culture | `{{ date \| date: "%x" }}` (short date) |
|---------|------------------------------------------|
| `en-US` | 01/15/24 |
| `en-GB` | 15/01/24 |
| `de-DE` | 15.01.24 |

!!! note
    For consistent date output across cultures, use explicit format strings like `"%Y-%m-%d"` instead of locale-dependent formats like `"%x"`.

## Common Cultures

| Code | Language/Region | Currency |
|------|-----------------|----------|
| `en-US` | English (United States) | $ USD |
| `en-GB` | English (United Kingdom) | £ GBP |
| `en-AU` | English (Australia) | $ AUD |
| `en-CA` | English (Canada) | $ CAD |
| `de-DE` | German (Germany) | € EUR |
| `fr-FR` | French (France) | € EUR |
| `es-ES` | Spanish (Spain) | € EUR |
| `it-IT` | Italian (Italy) | € EUR |
| `nl-NL` | Dutch (Netherlands) | € EUR |
| `pt-BR` | Portuguese (Brazil) | R$ BRL |
| `ja-JP` | Japanese (Japan) | ¥ JPY |
| `zh-CN` | Chinese (China) | ¥ CNY |

## Examples

### Multi-Currency Invoice

```liquid
{% assign culture = customer.country | default: "en-US" %}
<!-- Culture set via API based on customer country -->

<table>
    {% for item in invoice.lines %}
    <tr>
        <td>{{ item.description }}</td>
        <td>{{ item.quantity }}</td>
        <td>{{ item.unitPrice | format_number: "C" }}</td>
        <td>{{ item.total | format_number: "C" }}</td>
    </tr>
    {% endfor %}
</table>

<p class="total">
    <strong>Total:</strong> {{ invoice.total | format_number: "C" }}
</p>
```

### Explicit Currency Symbol

If you need a specific currency regardless of culture:

```liquid
<!-- Always show USD -->
${{ price | format_number: "N2" }}

<!-- Always show EUR -->
€{{ price | format_number: "N2" }}

<!-- Always show GBP -->
£{{ price | format_number: "N2" }}
```

### Regional Number Display

```liquid
<!-- Using culture-aware formatting -->
<p>Population: {{ stats.population | format_number: "N0" }}</p>
<!-- en-US: 1,234,567 -->
<!-- de-DE: 1.234.567 -->
```

## Content Blocks and Culture

Content blocks inherit the culture from the parent template context:

```liquid
<!-- Main template (culture: de-DE) -->
{{ contentBlock("cb_pricing", "price1", "product") }}

<!-- Content block will format as de-DE -->
<p>{{ price | format_number: "C" }}</p>
<!-- Output: 99,99 € -->
```

## Best Practices

1. **Set culture at render time** for dynamic multi-region support
2. **Use explicit formats** for dates when consistency matters
3. **Test with multiple cultures** if your templates serve international users
4. **Consider right-to-left** cultures if supporting Arabic, Hebrew, etc.

## Troubleshooting

### Wrong Currency Symbol

- Verify the culture code is correct
- Check that `cultureName` is being passed in the API request
- Default is typically `en-US` if not specified

### Numbers Not Formatting

- Ensure the value is a number, not a string
- Use `| to_number` to convert strings first

### Unexpected Decimal Separator

- This is correct for the culture (e.g., comma in European cultures)
- Use explicit format if you need consistent output: `| format_number: "F2"`

## See Also

- [Filters & Tags](filters-tags.md) - `format_number` and `parse_date`
- [REST API](../../developer-guide/rest-api.md) - Setting culture in API requests
- [.NET Culture Codes](https://learn.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo) - Full list of supported cultures
