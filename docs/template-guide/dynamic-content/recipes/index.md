# Recipes & Tutorials

Learn by building real templates. These tutorials walk you through complete examples from start to finish.

## Getting Started

<div class="grid cards" markdown>

-   :material-receipt:{ .lg .middle } **Invoice Template**

    ---

    Build a complete invoice with header, line items, and totals

    [:octicons-arrow-right-24: Start Tutorial](invoice-template.md)

-   :material-table:{ .lg .middle } **Iteration Patterns**

    ---

    Tables, lists, cards, and grids from array data

    [:octicons-arrow-right-24: View Patterns](iteration-patterns.md)

-   :material-code-braces:{ .lg .middle } **Conditional Content**

    ---

    Show/hide sections based on data

    [:octicons-arrow-right-24: Learn Conditionals](conditional-content.md)

</div>

## Quick Recipes

### Display a List of Items

```liquid
<ul>
{% for item in items %}
    <li>{{ item.name }} - {{ item.price | format_number: "C" }}</li>
{% endfor %}
</ul>
```

### Format a Date

```liquid
{{ order.date | parse_date | date: "%B %d, %Y" }}
<!-- Output: January 15, 2024 -->
```

### Show Content Conditionally

```liquid
{% if customer.isPremium %}
    <span class="badge">Premium Member</span>
{% endif %}
```

### Calculate a Total

```liquid
{% assign subtotal = 0 %}
{% for item in order.items %}
    {% assign lineTotal = item.price | times: item.quantity %}
    {% assign subtotal = subtotal | plus: lineTotal %}
{% endfor %}
<p>Subtotal: {{ subtotal | format_number: "C" }}</p>
```

### Add Page Numbers (PDF)

In your footer template:
```html
<div style="text-align: center; font-size: 9px;">
    Page {{tt_pageNumber}} of {{tt_totalPages}}
</div>
```

### Generate a QR Code

```liquid
{{ qrCode(order.trackingUrl, "#000", "#fff", 10, 150, 150) }}
```

### Number Sections Automatically

```liquid
<h2>{% secUp main %}. Introduction</h2>
<h2>{% secUp main %}. Methods</h2>
<h2>{% secUp main %}. Results</h2>
```

## Learning Path

**Beginner:**

1. [Variables](../variables.md) - Basic placeholders
2. [Invoice Tutorial](invoice-template.md) - Your first complete template
3. [Iteration Patterns](iteration-patterns.md) - Working with lists

**Intermediate:**

1. [Data Binding](../data-binding.md) - Complex data structures
2. [Filters & Tags](../filters-tags.md) - Formatting and logic
3. [Conditional Content](conditional-content.md) - Dynamic sections

**Advanced:**

1. [repeat() Function](../functions/repeat.md) - JSONPath iteration
2. [Content Blocks](../functions/content-blocks.md) - Reusable components
3. [Localization](../localization.md) - Multi-region support

## Common Tasks

| Task | Solution |
|------|----------|
| Display JSON data | `{{ variable }}` or `{{ object.property }}` |
| Loop through array | `{% for item in items %}...{% endfor %}` |
| Format currency | `{{ price \| format_number: "C" }}` |
| Format date | `{{ date \| parse_date \| date: "%Y-%m-%d" }}` |
| Show if exists | `{% if variable %}...{% endif %}` |
| Show if empty | `{% if items.size == 0 %}...{% endif %}` |
| Add page numbers | `{{tt_pageNumber}}` in PDF header/footer |
| Number sections | `{% secUp sectionName %}` |
| Generate QR code | `{{ qrCode(content) }}` |
| Reuse components | `{{ contentBlock(id, compId, scope) }}` |

## See Also

- [Debugging](../debugging.md) - Troubleshooting tips
- [Liquid Reference](../liquid-reference.md) - Complete syntax guide
- [Functions](../functions/index.md) - Advanced features
