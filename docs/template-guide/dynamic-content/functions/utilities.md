# Utility Functions

TemplateTo provides helper functions for common template tasks.

## isArray()

Checks whether a value is an array. Returns `true` or `false`.

### Syntax

```liquid
{% if isArray(value) %}
    ...
{% endif %}
```

### Why Use isArray()?

When your data structure might be either a single object or an array, `isArray()` lets you handle both cases:

**Data might be:**
```json
// Single item
{ "order": { "name": "Widget", "qty": 1 } }

// Or multiple items
{ "order": [
    { "name": "Widget", "qty": 1 },
    { "name": "Gadget", "qty": 2 }
] }
```

**Template:**
```liquid
{% if isArray(order) %}
    {% for item in order %}
        <div>{{ item.name }}: {{ item.qty }}</div>
    {% endfor %}
{% else %}
    <div>{{ order.name }}: {{ order.qty }}</div>
{% endif %}
```

### Common Use Cases

#### Dynamic List vs Single Item

```liquid
{% if isArray(recipients) %}
    <h3>Recipients:</h3>
    <ul>
        {% for person in recipients %}
            <li>{{ person.name }} &lt;{{ person.email }}&gt;</li>
        {% endfor %}
    </ul>
{% else %}
    <h3>Recipient:</h3>
    <p>{{ recipients.name }} &lt;{{ recipients.email }}&gt;</p>
{% endif %}
```

#### Conditional Table Rendering

```liquid
{% if isArray(data.items) and data.items.size > 0 %}
    <table>
        <thead>
            <tr><th>Item</th><th>Price</th></tr>
        </thead>
        <tbody>
            {% for item in data.items %}
            <tr>
                <td>{{ item.name }}</td>
                <td>{{ item.price | format_number: "C" }}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
{% elsif data.items %}
    <p>Single item: {{ data.items.name }}</p>
{% else %}
    <p>No items.</p>
{% endif %}
```

#### API Response Handling

APIs sometimes return different structures based on result count:

```liquid
{% if isArray(response.results) %}
    Found {{ response.results.size }} results
{% else %}
    Single result: {{ response.results.title }}
{% endif %}
```

### Combining with Other Checks

```liquid
{% if items and isArray(items) and items.size > 0 %}
    <!-- Safe to iterate -->
    {% for item in items %}...{% endfor %}
{% endif %}
```

### How It Works

`isArray()` checks the underlying Fluid value type:

| Value | isArray() returns |
|-------|-------------------|
| `[]` (empty array) | `true` |
| `[1, 2, 3]` | `true` |
| `[{...}, {...}]` | `true` |
| `{}` (object) | `false` |
| `"string"` | `false` |
| `123` | `false` |
| `null` | `false` |

### Troubleshooting

#### "isArray not found" error

Ensure you're using the function in a conditional, not as output:

```liquid
<!-- Wrong -->
{{ isArray(items) }}

<!-- Right -->
{% if isArray(items) %}...{% endif %}
```

#### Always returns false

- Check that the property path is correct
- The property might not exist (returns false for undefined)
- Use `{{ items | json }}` to inspect the actual structure

## Type Checking Alternatives

For simple checks, you can also use Liquid's built-in properties:

```liquid
<!-- Check if has items -->
{% if items.size > 0 %}...{% endif %}

<!-- Check if exists and not blank -->
{% if items != blank %}...{% endif %}

<!-- Check first item exists -->
{% if items.first %}...{% endif %}
```

However, these don't distinguish between an array and an object with a `size` property.

## See Also

- [Functions Overview](index.md) - All available functions
- [Data Binding](../data-binding.md) - Working with arrays
- [Liquid Reference](../liquid-reference.md) - Conditionals and operators
