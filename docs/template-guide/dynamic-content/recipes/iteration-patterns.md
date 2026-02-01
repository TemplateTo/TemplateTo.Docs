# Iteration Patterns

Common patterns for displaying array data as tables, lists, cards, and grids.

## Basic Table

The most common pattern - display array data in a table:

**Data:**
```json
{
  "products": [
    { "name": "Widget", "sku": "WGT-001", "price": 29.99, "stock": 150 },
    { "name": "Gadget", "sku": "GDG-002", "price": 49.99, "stock": 75 },
    { "name": "Gizmo", "sku": "GZM-003", "price": 19.99, "stock": 200 }
  ]
}
```

**Template:**
```html
<table>
    <thead>
        <tr>
            <th>SKU</th>
            <th>Product</th>
            <th>Price</th>
            <th>Stock</th>
        </tr>
    </thead>
    <tbody>
        {% for product in products %}
        <tr>
            <td>{{ product.sku }}</td>
            <td>{{ product.name }}</td>
            <td>{{ product.price | format_number: "C" }}</td>
            <td>{{ product.stock }}</td>
        </tr>
        {% endfor %}
    </tbody>
</table>
```

## Table with Row Numbers

Add row numbers using `forloop.index`:

```html
<tbody>
    {% for item in items %}
    <tr>
        <td>{{ forloop.index }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.value }}</td>
    </tr>
    {% endfor %}
</tbody>
```

## Table with Alternating Rows

Style odd/even rows differently:

```html
{% for item in items %}
<tr class="{% if forloop.index | modulo: 2 == 0 %}even{% else %}odd{% endif %}">
    <td>{{ item.name }}</td>
</tr>
{% endfor %}
```

**CSS:**
```css
tr.odd { background: #f9f9f9; }
tr.even { background: #ffffff; }
```

## Table with Row Totals

Calculate and display totals per row:

```html
{% for line in order.lines %}
<tr>
    <td>{{ line.description }}</td>
    <td>{{ line.quantity }}</td>
    <td>{{ line.unitPrice | format_number: "C" }}</td>
    <td>{{ line.quantity | times: line.unitPrice | format_number: "C" }}</td>
</tr>
{% endfor %}
```

## Simple List

**Unordered list:**
```html
<ul>
    {% for item in items %}
    <li>{{ item.name }}</li>
    {% endfor %}
</ul>
```

**Ordered list:**
```html
<ol>
    {% for step in steps %}
    <li>{{ step.instruction }}</li>
    {% endfor %}
</ol>
```

## Definition List

For key-value pairs:

```html
<dl>
    {% for field in fields %}
    <dt>{{ field.label }}</dt>
    <dd>{{ field.value }}</dd>
    {% endfor %}
</dl>
```

## Comma-Separated List

Join items inline:

```html
<p>Tags: {{ tags | join: ", " }}</p>

<!-- Or with more control -->
<p>Attendees:
{% for person in attendees %}
    {{ person.name }}{% unless forloop.last %}, {% endunless %}
{% endfor %}
</p>
```

## Card Grid

Display items as cards in a flexible grid:

**Template:**
```html
<div class="card-grid">
    {% for product in products %}
    <div class="card">
        <h3>{{ product.name }}</h3>
        <p class="price">{{ product.price | format_number: "C" }}</p>
        <p>{{ product.description }}</p>
    </div>
    {% endfor %}
</div>
```

**CSS:**
```css
.card-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
}

.card {
    border: 1px solid #ddd;
    padding: 15px;
    border-radius: 8px;
}
```

## Two-Column Layout

Split items into two columns:

```html
<div class="two-columns">
    <div class="column">
        {% for item in items limit: items.size | divided_by: 2 %}
        <p>{{ item.name }}</p>
        {% endfor %}
    </div>
    <div class="column">
        {% for item in items offset: items.size | divided_by: 2 %}
        <p>{{ item.name }}</p>
        {% endfor %}
    </div>
</div>
```

## Nested Loops

Iterate over nested arrays:

**Data:**
```json
{
  "categories": [
    {
      "name": "Electronics",
      "items": [
        { "name": "Phone", "price": 699 },
        { "name": "Laptop", "price": 1299 }
      ]
    },
    {
      "name": "Books",
      "items": [
        { "name": "Novel", "price": 15 },
        { "name": "Textbook", "price": 85 }
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
    {% for item in category.items %}
    <li>{{ item.name }} - {{ item.price | format_number: "C" }}</li>
    {% endfor %}
</ul>
{% endfor %}
```

## Grouped Items

Group items by a property:

**Data:**
```json
{
  "orders": [
    { "status": "pending", "id": "001", "total": 150 },
    { "status": "shipped", "id": "002", "total": 200 },
    { "status": "pending", "id": "003", "total": 75 },
    { "status": "delivered", "id": "004", "total": 300 }
  ]
}
```

**Using repeat() with JSONPath filtering:**
```html
<h3>Pending Orders</h3>
{{ repeat("$.orders[?(@.status=='pending')]",
    "<p>Order {{id}}: {{total | format_number: 'C'}}</p>",
    "") }}

<h3>Shipped Orders</h3>
{{ repeat("$.orders[?(@.status=='shipped')]",
    "<p>Order {{id}}: {{total | format_number: 'C'}}</p>",
    "") }}
```

## Empty State

Handle empty arrays gracefully:

```html
{% for item in items %}
<div class="item">{{ item.name }}</div>
{% else %}
<div class="empty-state">
    <p>No items to display.</p>
</div>
{% endfor %}
```

## Limited Display

Show only a subset of items:

```html
<!-- First 5 items -->
<h3>Top 5 Products</h3>
{% for product in products limit: 5 %}
<p>{{ product.name }}</p>
{% endfor %}

<!-- Show "and X more" -->
{% if products.size > 5 %}
<p class="more">and {{ products.size | minus: 5 }} more...</p>
{% endif %}
```

## Pagination Pattern

For multi-page documents with many items:

```html
{% for item in items %}
<div class="item">
    {{ item.name }}
</div>

<!-- Page break every 10 items -->
{% assign remainder = forloop.index | modulo: 10 %}
{% if remainder == 0 and forloop.last == false %}
<div style="page-break-after: always;"></div>
{% endif %}
{% endfor %}
```

## Summary Statistics

Show count and totals:

```html
<p>Total items: {{ items.size }}</p>

{% assign total = 0 %}
{% for item in items %}
    {% assign total = total | plus: item.price %}
{% endfor %}
<p>Total value: {{ total | format_number: "C" }}</p>
```

## Conditional Row Styling

Highlight specific rows:

```html
{% for order in orders %}
<tr class="{% if order.priority == 'high' %}highlight{% endif %} {% if order.overdue %}overdue{% endif %}">
    <td>{{ order.id }}</td>
    <td>{{ order.customer }}</td>
    <td>{{ order.total | format_number: "C" }}</td>
</tr>
{% endfor %}
```

**CSS:**
```css
tr.highlight { background: #fff3cd; }
tr.overdue { color: #dc3545; }
```

## See Also

- [Data Binding](../data-binding.md) - Array access and iteration
- [repeat() Function](../functions/repeat.md) - JSONPath filtering
- [Invoice Template](invoice-template.md) - Complete table example
- [Conditional Content](conditional-content.md) - Show/hide patterns
