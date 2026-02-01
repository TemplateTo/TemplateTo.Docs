# Data Binding

This guide covers how to work with JSON data in your templates, including nested objects, arrays, and common patterns.

## JSON Data Structure

Data is passed to templates as JSON. Understanding the structure helps you access values correctly.

```json
{
  "simple": "value",
  "nested": {
    "property": "value"
  },
  "array": [
    { "name": "Item 1" },
    { "name": "Item 2" }
  ]
}
```

## Accessing Data

### Simple Properties

Access top-level properties directly:

```liquid
{{ simple }}
<!-- Result: value -->
```

### Nested Properties

Use dot notation to access nested objects:

```liquid
{{ nested.property }}
<!-- Result: value -->

{{ customer.address.city }}
{{ order.items.first.name }}
```

### Array Access

Access array items by index (zero-based):

```liquid
{{ array[0].name }}
<!-- Result: Item 1 -->

{{ array.first.name }}
<!-- Result: Item 1 -->

{{ array.last.name }}
<!-- Result: Item 2 -->

{{ array.size }}
<!-- Result: 2 -->
```

## Iterating Over Arrays

### Basic Loop

Use `for` to iterate over arrays:

```liquid
{% for item in order.lines %}
<tr>
    <td>{{ item.name }}</td>
    <td>{{ item.quantity }}</td>
    <td>{{ item.price }}</td>
</tr>
{% endfor %}
```

### Loop Variables

Liquid provides special variables inside loops:

| Variable | Description |
|----------|-------------|
| `forloop.index` | Current iteration (1-based) |
| `forloop.index0` | Current iteration (0-based) |
| `forloop.first` | `true` if first iteration |
| `forloop.last` | `true` if last iteration |
| `forloop.length` | Total number of items |

```liquid
{% for item in items %}
<tr class="{% if forloop.first %}first-row{% endif %}">
    <td>{{ forloop.index }}</td>
    <td>{{ item.name }}</td>
</tr>
{% endfor %}
```

### Loop with Limit and Offset

Control which items to display:

```liquid
<!-- First 5 items -->
{% for item in items limit:5 %}
    {{ item.name }}
{% endfor %}

<!-- Skip first 2, show next 3 -->
{% for item in items offset:2 limit:3 %}
    {{ item.name }}
{% endfor %}
```

### Empty Arrays

Handle empty arrays with `else`:

```liquid
{% for item in items %}
    <li>{{ item.name }}</li>
{% else %}
    <li>No items found</li>
{% endfor %}
```

## Conditional Data

### Checking if Data Exists

```liquid
{% if customer.phone %}
    <p>Phone: {{ customer.phone }}</p>
{% endif %}

{% if order.notes != blank %}
    <div class="notes">{{ order.notes }}</div>
{% endif %}
```

### Checking Array Contents

```liquid
{% if items.size > 0 %}
    <table>
        {% for item in items %}
            <tr><td>{{ item.name }}</td></tr>
        {% endfor %}
    </table>
{% else %}
    <p>No items to display.</p>
{% endif %}
```

### Type Checking with isArray()

Use `isArray()` to check if a value is an array:

```liquid
{% if isArray(data.items) %}
    {% for item in data.items %}
        {{ item.name }}
    {% endfor %}
{% else %}
    {{ data.items.name }}
{% endif %}
```

This is useful when data might be either a single object or an array.

## Common Data Patterns

### Invoice with Line Items

**JSON:**
```json
{
  "invoice": {
    "number": "INV-001",
    "date": "15/01/24",
    "customer": {
      "name": "Acme Corp",
      "address": "123 Main St"
    },
    "lines": [
      { "description": "Widget", "qty": 2, "price": 10.00 },
      { "description": "Gadget", "qty": 1, "price": 25.00 }
    ],
    "total": 45.00
  }
}
```

**Template:**
```html
<h1>Invoice {{ invoice.number }}</h1>
<p>Date: {{ invoice.date | parse_date | date: "%B %d, %Y" }}</p>

<h2>Bill To:</h2>
<p>{{ invoice.customer.name }}<br>
   {{ invoice.customer.address }}</p>

<table>
    <thead>
        <tr>
            <th>Description</th>
            <th>Qty</th>
            <th>Price</th>
            <th>Total</th>
        </tr>
    </thead>
    <tbody>
        {% for line in invoice.lines %}
        <tr>
            <td>{{ line.description }}</td>
            <td>{{ line.qty }}</td>
            <td>{{ line.price | format_number: "C" }}</td>
            <td>{{ line.qty | times: line.price | format_number: "C" }}</td>
        </tr>
        {% endfor %}
    </tbody>
    <tfoot>
        <tr>
            <td colspan="3">Total</td>
            <td>{{ invoice.total | format_number: "C" }}</td>
        </tr>
    </tfoot>
</table>
```

### Nested Arrays

**JSON:**
```json
{
  "categories": [
    {
      "name": "Electronics",
      "products": [
        { "name": "Laptop", "price": 999 },
        { "name": "Phone", "price": 699 }
      ]
    },
    {
      "name": "Books",
      "products": [
        { "name": "Novel", "price": 15 }
      ]
    }
  ]
}
```

**Template:**
```html
{% for category in categories %}
<h2>{{ category.name }}</h2>
<ul>
    {% for product in category.products %}
    <li>{{ product.name }} - {{ product.price | format_number: "C" }}</li>
    {% endfor %}
</ul>
{% endfor %}
```

### Optional Sections

**JSON:**
```json
{
  "showDisclaimer": true,
  "disclaimer": "Terms and conditions apply.",
  "specialOffer": null
}
```

**Template:**
```html
{% if showDisclaimer %}
<div class="disclaimer">
    {{ disclaimer }}
</div>
{% endif %}

{% if specialOffer %}
<div class="offer">{{ specialOffer }}</div>
{% endif %}
```

## Advanced: JSONPath with repeat()

For complex array filtering, use the `repeat()` function with JSONPath:

```liquid
{{ repeat("$.orders[?(@.status=='pending')]", "<li>{{name}}</li>", "<ul>_</ul>") }}
```

See [repeat() Function](functions/repeat.md) for details.

## Troubleshooting

### "Variable not found"

- Check the JSON path is correct
- Verify the data exists in your test JSON
- Check for typos in property names (case-sensitive)

### Empty output

- The property might be `null` or empty string
- Use `{% if variable %}` to check before displaying
- For numbers, `0` is falsy in Liquid conditionals

### Arrays not iterating

- Verify the data is actually an array
- Use `isArray()` to check: `{% if isArray(items) %}`
- Check the property path is correct

## See Also

- [Variables](variables.md) - Basic variable usage
- [Liquid Reference](liquid-reference.md) - Complete Liquid syntax
- [repeat() Function](functions/repeat.md) - JSONPath iteration
- [Debugging](debugging.md) - Troubleshooting tips
