# Functions

TemplateTo provides special functions that extend Liquid's capabilities for document generation.

## Quick Reference

| Function | Purpose | Example |
|----------|---------|---------|
| [`repeat()`](repeat.md) | Iterate with JSONPath | `{{ repeat("$.items[*]", "...", "...") }}` |
| [`contentBlock()`](content-blocks.md) | Embed reusable blocks | `{{ contentBlock("cb_123", "comp1", "All") }}` |
| [`qrCode()`](qr-codes-barcodes.md) | Generate QR codes | `{{ qrCode(url) }}` |
| [`barcode()`](qr-codes-barcodes.md) | Generate barcodes | `{{ barcode(code, "CODE128") }}` |
| [`isArray()`](utilities.md) | Check if value is array | `{% if isArray(items) %}` |

## Function Syntax

Functions use Liquid's output syntax with parentheses:

```liquid
{{ functionName(argument1, argument2) }}
```

Functions can also be used in control flow:

```liquid
{% if isArray(data) %}
    {% for item in data %}...{% endfor %}
{% endif %}
```

## When to Use Functions

### repeat() vs for loops

Use standard `{% for %}` loops when:
- Iterating over a simple array
- You need full Liquid control flow

Use `repeat()` when:
- You need JSONPath filtering
- The array is deeply nested
- You want to filter items with JSONPath expressions

### contentBlock() for reusability

Use `contentBlock()` when:
- Multiple templates share the same component
- You want centralized styling/content updates
- Building modular document systems

### qrCode() and barcode()

Use these when:
- Generating trackable documents
- Creating shipping labels
- Adding scannable links or references

## Functions in Detail

<div class="grid cards" markdown>

-   :material-repeat:{ .lg .middle } **repeat()**

    ---

    JSONPath-based iteration for advanced data filtering

    [:octicons-arrow-right-24: Learn more](repeat.md)

-   :material-puzzle:{ .lg .middle } **contentBlock()**

    ---

    Embed reusable content blocks with data scoping

    [:octicons-arrow-right-24: Learn more](content-blocks.md)

-   :material-qrcode:{ .lg .middle } **qrCode() & barcode()**

    ---

    Generate QR codes and barcodes inline

    [:octicons-arrow-right-24: Learn more](qr-codes-barcodes.md)

-   :material-code-brackets:{ .lg .middle } **Utilities**

    ---

    Helper functions like isArray()

    [:octicons-arrow-right-24: Learn more](utilities.md)

</div>

## Combining Functions

Functions can be combined with standard Liquid:

```liquid
{% if showQRCode %}
    {{ qrCode(trackingUrl, "#000", "#fff", 10, 150, 150) }}
{% endif %}

{% if isArray(order.lines) %}
    {{ repeat("$.order.lines[*]", "<tr><td>{{name}}</td></tr>", "<table>_</table>") }}
{% else %}
    <p>Single item: {{ order.lines.name }}</p>
{% endif %}
```

## See Also

- [Liquid Reference](../liquid-reference.md) - Standard Liquid syntax
- [Filters & Tags](../filters-tags.md) - TemplateTo filters and tags
- [Data Binding](../data-binding.md) - Working with JSON data
