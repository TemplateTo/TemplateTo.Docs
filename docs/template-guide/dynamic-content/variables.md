# Working with Variables

Variables let you create templates that generate personalized documents. Instead of hardcoding values, you use placeholders that get replaced with real data at render time.

## The Basics

Variables use double curly braces: `{{variableName}}`

When you render a template, you send JSON data. TemplateTo replaces each variable with the corresponding value from your data.

**Template:**
```html
Hello, {{name}}!
```

**Data:**
```json
{
  "name": "Alice"
}
```

**Result:**
```html
Hello, Alice!
```

## Adding Test Data

In the template editor, paste your JSON into the **Template Data** panel to test your template.

![JSON data editor](../../images/dbee0b3184aa36ff0628bbed2fc059951c1f8289072cbe6072a3dcdfa241bf15.png)

??? example "Sample JSON Data"
    ```json
    {
      "customerInvoice": {
        "name": "Lynx",
        "total": "123",
        "lines": [
          { "sku": "qwe123", "qnt": "2", "name": "Fragrance", "total": "82" },
          { "sku": "qwe124", "qnt": "1", "name": "Fragrance Bold", "total": "41" }
        ],
        "date": "15/11/23"
      },
      "email": "david@templateto.com",
      "country": "UK"
    }
    ```

For simple key-value data, use the **Create** button to add variables without writing JSON manually.

![Create Template data](../../images/8df6569c03fe8bbde759696128bbac4c0299a1b3583a7cba97feb6a337a7e23a.png)

## Using Variables in Templates

### From the Editor

Use the variable dropdown to insert variables without typing:

![variables from dropdown](../../images/8988e8cf1b07cfb9d5502ce271ab950cb86a5b45310b98522551275ecb606dfa.png)

### By Typing

Type the variable name directly in your HTML:

```html
<p>Customer: {{customerInvoice.name}}</p>
<p>Email: {{email}}</p>
```

## Accessing Nested Data

Use dot notation to access nested properties:

```html
<!-- Access nested object properties -->
{{customerInvoice.name}}
{{customerInvoice.total}}
{{customerInvoice.date}}
```

For more complex data access patterns including arrays, see [Data Binding](data-binding.md).

## Liquid Templating

TemplateTo uses the [Liquid](https://shopify.github.io/liquid/) templating engine. This gives you:

- **Variables**: `{{variable}}`
- **Logic**: `{% if condition %}...{% endif %}`
- **Loops**: `{% for item in collection %}...{% endfor %}`
- **Filters**: `{{value | filter}}`

See [Liquid Reference](liquid-reference.md) for complete syntax documentation.

## Computed Variables

For complex transformations, create **Liquid Variables** in the editor:

![Liquid variable editing](../../images/c9cd24ad30bed6ac9e9666814b37d81cb126a1e53800c7dd573dbbc8823df940.png)

1. Navigate to the **Data** tab in the editor
2. Give your variable a name
3. Write your Liquid expression

!!! warning "Variable Order"
    If one variable uses another, define the dependency first. Variables are processed in order.

**Example:** Create a computed variable called `formattedDate`:
```liquid
{{ customerInvoice.date | parse_date | date: "%e %B %Y" }}
```

Then use it in your template:
```html
<p>Invoice Date: {{formattedDate}}</p>
```

## Next Steps

- [Data Binding](data-binding.md) - Learn about arrays, nested access, and JSON patterns
- [Filters & Tags](filters-tags.md) - Format dates, numbers, and more
- [Liquid Reference](liquid-reference.md) - Complete Liquid syntax guide
