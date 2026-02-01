# Debugging Templates

This guide helps you troubleshoot common template issues and understand what's happening with your data.

## Common Issues

### Variables Not Displaying

**Symptom:** `{{variable}}` appears as blank or the literal text `{{variable}}`

**Causes and solutions:**

1. **Typo in variable name** (case-sensitive)
   ```liquid
   <!-- Wrong -->
   {{ CustomerName }}

   <!-- Right (if JSON has "customerName") -->
   {{ customerName }}
   ```

2. **Wrong path to nested data**
   ```liquid
   <!-- Wrong -->
   {{ customer.address }}

   <!-- Right (if address is nested deeper) -->
   {{ customer.billing.address }}
   ```

3. **Data not provided** - Check your test JSON contains the expected property

4. **Liquid syntax error** - Missing closing braces or filter issues
   ```liquid
   <!-- Wrong -->
   {{ name | upcase }

   <!-- Right -->
   {{ name | upcase }}
   ```

### Array Not Iterating

**Symptom:** `{% for %}` loop produces no output

**Solutions:**

1. **Check if data is actually an array**
   ```liquid
   {% if isArray(items) %}
       Array with {{ items.size }} items
   {% else %}
       Not an array
   {% endif %}
   ```

2. **Verify the path is correct**
   ```liquid
   <!-- If items are nested -->
   {% for item in order.lineItems %}
       {{ item.name }}
   {% endfor %}
   ```

3. **Check for empty array**
   ```liquid
   {% for item in items %}
       {{ item.name }}
   {% else %}
       No items found
   {% endfor %}
   ```

### Numbers Not Formatting

**Symptom:** `format_number` not working or showing unexpected results

**Solutions:**

1. **Value might be a string** - Convert first
   ```liquid
   {{ "123.45" | to_number | format_number: "C" }}
   ```

2. **Wrong format specifier**
   ```liquid
   <!-- N = number, C = currency, P = percentage -->
   {{ 0.25 | format_number: "P" }}  <!-- 25.00% -->
   {{ 100 | format_number: "C" }}   <!-- $100.00 -->
   ```

### Dates Not Parsing

**Symptom:** `parse_date` returns nothing or wrong date

**Solutions:**

1. **Check date format** - European (DD/MM/YY) is default
   ```liquid
   <!-- European format (default) -->
   {{ "15/01/24" | parse_date | date: "%Y-%m-%d" }}

   <!-- US format - pass true -->
   {{ "01/15/24" | parse_date: true | date: "%Y-%m-%d" }}
   ```

2. **Date might already be a date object** - Try without `parse_date`

## Debugging Techniques

### Inspect Data Structure

Use JSON output to see what data is available:

```liquid
<pre>{{ data | json }}</pre>
```

This shows the raw JSON structure, helping you verify paths.

### Check Variable Type

```liquid
Type: {{ variable | type }}
Size: {{ variable | size }}
Is Array: {% if isArray(variable) %}Yes{% else %}No{% endif %}
```

### Debug with Conditionals

```liquid
{% if customer %}
    Customer exists: {{ customer.name }}
{% else %}
    Customer is null/undefined
{% endif %}

{% if items.size > 0 %}
    Has {{ items.size }} items
{% else %}
    Items is empty
{% endif %}
```

### tt_dumpData in Content Blocks

When using `contentBlock()`, the special placeholder `{{tt_dumpData}}` shows the data available to that content block:

```html
<!-- Inside a content block -->
<h2>Debug: Data Available</h2>
{{tt_dumpData}}
```

This renders a `<pre>` tag with prettified JSON of the scoped data.

!!! note
    `{{tt_dumpData}}` only works inside content blocks, not in the main template.

### Step-by-Step Isolation

When a complex expression doesn't work, break it down:

```liquid
<!-- Instead of this -->
{{ order.lines | map: "total" | sum | format_number: "C" }}

<!-- Try step by step -->
Lines: {{ order.lines | json }}
Totals: {{ order.lines | map: "total" | json }}
Sum: {{ order.lines | map: "total" | sum }}
```

## Error Messages

### "A value was expected at..."

A variable or expression is invalid at the specified position.

```
Error: A value was expected at (5:23)
```

Check line 5, position 23 for:
- Missing variable name
- Invalid filter syntax
- Unclosed brackets

### "End of tag '}}' was expected at..."

Missing closing braces:

```liquid
<!-- Wrong -->
{{ name | upcase

<!-- Right -->
{{ name | upcase }}
```

### Template Validation

Use the API's validate endpoint or check in the editor. Syntax errors will be highlighted.

## Testing Strategies

### Start Simple

Begin with a minimal template and add complexity:

```liquid
<!-- Step 1: Basic variable -->
{{ customer.name }}

<!-- Step 2: Add formatting -->
{{ customer.name | upcase }}

<!-- Step 3: Add conditional -->
{% if customer.isPremium %}Premium{% endif %}: {{ customer.name | upcase }}
```

### Use Sample Data

Create minimal test JSON that covers your cases:

```json
{
  "customer": {
    "name": "Test Customer",
    "isPremium": true
  },
  "items": [
    { "name": "Item 1", "price": 10 },
    { "name": "Item 2", "price": 20 }
  ],
  "emptyArray": [],
  "nullValue": null
}
```

### Test Edge Cases

- Empty arrays
- Null/missing values
- Zero values
- Very long strings
- Special characters

## Performance Issues

### Large Arrays

For arrays with 100+ items, consider:
- Pagination in your data source
- Using `limit` in for loops: `{% for item in items limit:50 %}`

### Complex Nested Loops

Deeply nested loops can slow rendering:

```liquid
<!-- Potentially slow with large datasets -->
{% for category in categories %}
    {% for product in category.products %}
        {% for variant in product.variants %}
            ...
        {% endfor %}
    {% endfor %}
{% endfor %}
```

Consider restructuring your data to flatten nested structures.

## Getting Help

If you're stuck:

1. Check this documentation for the feature you're using
2. Verify your JSON structure matches your template expectations
3. Test with minimal example that reproduces the issue
4. Check the [Liquid documentation](https://shopify.github.io/liquid/) for standard features

## See Also

- [Variables](variables.md) - Basic variable usage
- [Data Binding](data-binding.md) - Working with JSON data
- [Liquid Reference](liquid-reference.md) - Liquid syntax
- [Filters & Tags](filters-tags.md) - TemplateTo-specific features
