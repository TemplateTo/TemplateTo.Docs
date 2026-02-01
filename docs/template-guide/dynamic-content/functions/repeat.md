# repeat() Function

The `repeat()` function provides powerful JSONPath-based iteration, allowing you to filter and iterate over arrays using JSONPath expressions.

## Syntax

```liquid
{{ repeat(jsonPath, itemTemplate, wrapperTemplate) }}
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `jsonPath` | string | Yes | JSONPath expression to select items |
| `itemTemplate` | string | Yes | Template rendered for each item |
| `wrapperTemplate` | string | No | Wrapper around all items (use `_` as placeholder) |

## Basic Example

**JSON Data:**
```json
{
  "products": [
    { "name": "Widget", "price": 10 },
    { "name": "Gadget", "price": 25 },
    { "name": "Gizmo", "price": 15 }
  ]
}
```

**Template:**
```liquid
{{ repeat("$.products[*]", "<li>{{name}} - ${{price}}</li>", "<ul>_</ul>") }}
```

**Output:**
```html
<ul>
    <li>Widget - $10</li>
    <li>Gadget - $25</li>
    <li>Gizmo - $15</li>
</ul>
```

## JSONPath Syntax

JSONPath is like XPath for JSON. Here are common patterns:

| Expression | Description |
|------------|-------------|
| `$.property` | Root property |
| `$.obj.nested` | Nested property |
| `$.array[*]` | All array items |
| `$.array[0]` | First array item |
| `$.array[-1]` | Last array item |
| `$.array[0:3]` | Items 0, 1, 2 |
| `$..property` | Recursive descent (find all) |
| `$.array[?(@.active)]` | Filter: where active is truthy |
| `$.array[?(@.price > 10)]` | Filter: where price > 10 |

## Filtering with JSONPath

### Filter by Property Value

```liquid
<!-- Only active items -->
{{ repeat("$.items[?(@.active == true)]", "<li>{{name}}</li>", "<ul>_</ul>") }}

<!-- Items with status 'pending' -->
{{ repeat("$.orders[?(@.status == 'pending')]", "<tr><td>{{id}}</td></tr>", "<table>_</table>") }}
```

### Filter by Numeric Comparison

```liquid
<!-- Products over $50 -->
{{ repeat("$.products[?(@.price > 50)]", "<div>{{name}}: ${{price}}</div>", "") }}

<!-- Orders with quantity >= 10 -->
{{ repeat("$.orders[?(@.qty >= 10)]", "{{id}}, ", "") }}
```

### Combine Filters

```liquid
<!-- Active products under $100 -->
{{ repeat("$.products[?(@.active && @.price < 100)]", "<li>{{name}}</li>", "<ul>_</ul>") }}
```

## Item Template Context

Inside the item template, you can access:

- Direct properties: `{{propertyName}}`
- The current item as `_`: `{{_}}`
- Nested properties: `{{nested.property}}`

**Example with nested data:**

```json
{
  "orders": [
    {
      "id": "ORD-001",
      "customer": { "name": "Alice", "email": "alice@example.com" },
      "total": 150
    }
  ]
}
```

```liquid
{{ repeat("$.orders[*]",
    "<div class='order'>
        <h3>Order {{id}}</h3>
        <p>Customer: {{customer.name}}</p>
        <p>Total: ${{total}}</p>
    </div>",
    "")
}}
```

## Wrapper Template

The third parameter wraps all rendered items. Use `_` as a placeholder for the content.

**Without wrapper:**
```liquid
{{ repeat("$.items[*]", "<li>{{name}}</li>", "") }}
<!-- Output: <li>A</li><li>B</li><li>C</li> -->
```

**With wrapper:**
```liquid
{{ repeat("$.items[*]", "<li>{{name}}</li>", "<ul class='my-list'>_</ul>") }}
<!-- Output: <ul class='my-list'><li>A</li><li>B</li><li>C</li></ul> -->
```

## Real-World Examples

### Building a Table

```liquid
{{ repeat("$.invoice.lines[*]",
    "<tr>
        <td>{{description}}</td>
        <td>{{quantity}}</td>
        <td>${{unitPrice}}</td>
        <td>${{total}}</td>
    </tr>",
    "<table class='invoice-lines'>
        <thead>
            <tr>
                <th>Description</th>
                <th>Qty</th>
                <th>Unit Price</th>
                <th>Total</th>
            </tr>
        </thead>
        <tbody>_</tbody>
    </table>")
}}
```

### Category Listing

```json
{
  "categories": [
    {
      "name": "Electronics",
      "products": [
        { "name": "Phone", "inStock": true },
        { "name": "Laptop", "inStock": false }
      ]
    }
  ]
}
```

```liquid
{% for category in categories %}
<h2>{{ category.name }}</h2>
{{ repeat("$.categories[" | append: forloop.index0 | append: "].products[?(@.inStock == true)]",
    "<li>{{name}} - In Stock</li>",
    "<ul>_</ul>") }}
{% endfor %}
```

### Badge/Tag List

```liquid
{{ repeat("$.tags[*]",
    "<span class='badge'>{{_}}</span>",
    "<div class='tags'>_</div>") }}
```

## Comparison with for Loops

| Feature | `{% for %}` | `repeat()` |
|---------|-------------|------------|
| Simple iteration | Yes | Yes |
| JSONPath filtering | No | Yes |
| Loop variables (index, first, last) | Yes | No |
| Nested loops | Yes | Limited |
| Wrapper syntax | Manual | Built-in |
| Liquid logic inside | Full | Limited |

**Use `{% for %}` when:**
- You need `forloop.index`, `forloop.first`, etc.
- You need complex Liquid logic inside the loop
- The data structure is simple

**Use `repeat()` when:**
- You need to filter items with JSONPath
- You want concise table/list generation
- The array is deeply nested

## Troubleshooting

### No output

- Verify the JSONPath is correct using a [JSONPath tester](https://jsonpath.com/)
- Check that the data matches your filter criteria
- Ensure the JSON property names are correct (case-sensitive)

### Partial output

- JSONPath filters might be excluding items
- Check filter conditions: `@.property == 'value'` (use single quotes for strings)

### HTML entities appearing

If you see `&lt;` instead of `<`, the output is being double-escaped. This shouldn't normally happen with `repeat()`.

## See Also

- [Data Binding](../data-binding.md) - Standard array iteration
- [Liquid Reference](../liquid-reference.md) - For loops and alternatives
- [JSONPath Documentation](https://goessner.net/articles/JsonPath/) - Full JSONPath specification
