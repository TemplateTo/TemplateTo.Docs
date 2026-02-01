# contentBlock() Function

The `contentBlock()` function embeds reusable content blocks into your templates. Content blocks are standalone HTML/CSS components that can be shared across multiple templates.

## Why Use Content Blocks?

- **Reusability**: Define a component once, use it in many templates
- **Consistency**: Update the block, and all templates using it get the changes
- **Data Scoping**: Pass specific data subsets to each block
- **Modular Design**: Build complex documents from smaller, testable pieces

## Syntax

```liquid
{{ contentBlock(blockId, componentId, dataScope, customData) }}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `blockId` | string | Yes | The content block ID (e.g., `"cb_abc123"`) |
| `componentId` | string | Yes | Unique ID for this instance (for CSS scoping) |
| `dataScope` | string | Yes | Data to pass: `"All"`, `"Custom"`, or a JSON path |
| `customData` | string | No | Custom JSON data (when dataScope is `"Custom"`) |

## Basic Example

**Content Block HTML (defined separately):**
```html
<div class="product-card">
    <h3>{{ name }}</h3>
    <p class="price">{{ price | format_number: "C" }}</p>
</div>
```

**Template using the block:**
```liquid
{{ contentBlock("cb_product123", "product1", "products[0]") }}
```

## Data Scoping Options

### "All" - Full Data Access

Pass all template data to the block:

```liquid
{{ contentBlock("cb_header", "header1", "All") }}
```

The block receives the entire JSON object and can access any property.

### JSON Path - Specific Data

Pass a specific portion of your data:

```liquid
<!-- Pass the customer object -->
{{ contentBlock("cb_address", "billTo", "customer.billingAddress") }}

<!-- Pass a specific array item -->
{{ contentBlock("cb_product", "prod1", "order.items[0]") }}
```

**JSON:**
```json
{
  "customer": {
    "billingAddress": {
      "street": "123 Main St",
      "city": "London"
    }
  }
}
```

**Content Block receives:**
```json
{
  "street": "123 Main St",
  "city": "London"
}
```

### "Custom" - Dynamic Data

Build custom data dynamically:

```liquid
{{ contentBlock("cb_summary", "summary1", "Custom", '{"total": "{{order.total}}", "count": "{{order.items.size}}"}') }}
```

The custom data JSON is processed through Liquid first, then passed to the block.

## Component ID (CSS Scoping)

The `componentId` ensures CSS from one block doesn't affect others:

```liquid
<!-- Two instances of the same block with different data -->
{{ contentBlock("cb_card", "card1", "products[0]") }}
{{ contentBlock("cb_card", "card2", "products[1]") }}
```

CSS in the content block is automatically scoped:

```css
/* In content block */
.card { background: white; }

/* Rendered as */
#card1 .card { background: white; }
#card2 .card { background: white; }
```

## Using Content Blocks with Arrays

### Manual Iteration

```liquid
{% for item in order.items %}
    {{ contentBlock("cb_lineItem", "line" | append: forloop.index, "order.items[" | append: forloop.index0 | append: "]") }}
{% endfor %}
```

### Array as Data Scope

When you pass an array path, the block renders once per item:

```liquid
{{ contentBlock("cb_row", "rows", "order.items") }}
```

If `order.items` is an array with 3 items, the block renders 3 times.

## Debugging with tt_dumpData

Inside a content block, use `{{tt_dumpData}}` to see what data is available:

**Content Block HTML:**
```html
<div class="debug">
    <h4>Available Data:</h4>
    {{tt_dumpData}}
</div>
<div class="content">
    <h3>{{ name }}</h3>
</div>
```

This renders the scoped data as prettified JSON in a `<pre>` tag.

!!! warning
    Remove `{{tt_dumpData}}` before production use. It's only for debugging.

## Real-World Examples

### Invoice Header Block

**Content Block (cb_invoiceHeader):**
```html
<div class="invoice-header">
    <div class="company">
        <img src="{{ company.logo }}" alt="{{ company.name }}">
        <h1>{{ company.name }}</h1>
    </div>
    <div class="invoice-details">
        <p><strong>Invoice:</strong> {{ invoice.number }}</p>
        <p><strong>Date:</strong> {{ invoice.date | parse_date | date: "%B %d, %Y" }}</p>
    </div>
</div>
```

**Template:**
```liquid
{{ contentBlock("cb_invoiceHeader", "header", "All") }}
```

### Address Block (Reused for Bill To / Ship To)

**Content Block (cb_address):**
```html
<div class="address">
    <p><strong>{{ name }}</strong></p>
    <p>{{ street }}</p>
    <p>{{ city }}, {{ state }} {{ zip }}</p>
    <p>{{ country }}</p>
</div>
```

**Template:**
```liquid
<div class="addresses">
    <div class="bill-to">
        <h3>Bill To:</h3>
        {{ contentBlock("cb_address", "billTo", "customer.billing") }}
    </div>
    <div class="ship-to">
        <h3>Ship To:</h3>
        {{ contentBlock("cb_address", "shipTo", "customer.shipping") }}
    </div>
</div>
```

### Product Card Grid

**Content Block (cb_productCard):**
```html
<div class="card">
    <img src="{{ image }}" alt="{{ name }}">
    <h4>{{ name }}</h4>
    <p class="price">{{ price | format_number: "C" }}</p>
    {% if onSale %}
        <span class="badge">Sale!</span>
    {% endif %}
</div>
```

**Template:**
```liquid
<div class="product-grid">
    {% for product in products %}
        {{ contentBlock("cb_productCard", "prod" | append: forloop.index, "products[" | append: forloop.index0 | append: "]") }}
    {% endfor %}
</div>
```

## Creating Content Blocks

Content blocks are created in the TemplateTo UI:

1. Go to **Templates** > **Content Blocks**
2. Click **Create Content Block**
3. Add your HTML and CSS
4. Note the block ID (shown in the URL or settings)

The block ID format is typically `cb_` followed by an identifier.

## Best Practices

1. **Keep blocks focused**: One component per block
2. **Use meaningful componentIds**: `"header"`, `"footer"`, `"productCard1"` instead of `"a"`, `"b"`
3. **Document data requirements**: Note what properties the block expects
4. **Test with tt_dumpData**: Verify the block receives expected data
5. **Consider CSS conflicts**: Use the component scoping feature

## Troubleshooting

### Block Not Rendering

- Verify the block ID is correct
- Check that the block exists in your account
- Ensure the block is associated with your template

### Wrong Data in Block

- Use `{{tt_dumpData}}` to see what data the block receives
- Check your data scope path
- Verify JSON paths are correct (case-sensitive)

### CSS Conflicts

- Ensure each instance has a unique componentId
- Avoid `body` selectors in block CSS (they're automatically removed)

## See Also

- [Functions Overview](index.md) - All available functions
- [Themes](../themes.md) - Sharing CSS across templates
- [Debugging](../debugging.md) - Troubleshooting tips
