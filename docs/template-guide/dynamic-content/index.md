# Dynamic Content

Make your templates data-driven with variables, Liquid templating, and powerful functions.

## Choose Your Path

<div class="grid cards" markdown>

-   :material-school:{ .lg .middle } **New to Templates?**

    ---

    Start here to learn the basics of using variables and data in your templates.

    [:octicons-arrow-right-24: Variables Basics](variables.md)

-   :material-code-braces:{ .lg .middle } **Know the Basics?**

    ---

    Learn about data binding patterns and Liquid syntax.

    [:octicons-arrow-right-24: Data Binding](data-binding.md)

-   :material-function:{ .lg .middle } **Need Advanced Features?**

    ---

    Explore functions like `repeat()`, `qrCode()`, and `contentBlock()`.

    [:octicons-arrow-right-24: Functions Reference](functions/index.md)

-   :material-book-open-variant:{ .lg .middle } **Learn by Example?**

    ---

    Follow step-by-step tutorials to build real templates.

    [:octicons-arrow-right-24: Recipes & Tutorials](recipes/index.md)

</div>

## Quick Reference

| Feature | Description | Documentation |
|---------|-------------|---------------|
| `{{variable}}` | Insert data values | [Variables](variables.md) |
| `{% if %}` / `{% for %}` | Logic and loops | [Liquid Reference](liquid-reference.md) |
| `parse_date`, `format_number` | Data formatting | [Filters & Tags](filters-tags.md) |
| `repeat()` | JSONPath iteration | [repeat()](functions/repeat.md) |
| `qrCode()`, `barcode()` | Generate codes | [QR & Barcodes](functions/qr-codes-barcodes.md) |
| `contentBlock()` | Reusable blocks | [Content Blocks](functions/content-blocks.md) |
| `{{tt_pageNumber}}` | Page numbers | [Page Placeholders](page-placeholders.md) |
| Culture settings | Regional formatting | [Localization](localization.md) |

## How Dynamic Content Works

1. **Add placeholders** in your template using `{{variableName}}` syntax
2. **Send data** as JSON when calling the API
3. **Receive personalized documents** with your data inserted

### Example

**Template:**
```html
<h1>Invoice for {{customer.name}}</h1>
<p>Amount due: {{customer.total | format_number: "C"}}</p>
```

**API Data:**
```json
{
  "customer": {
    "name": "Acme Corp",
    "total": 1500
  }
}
```

**Output:**
```html
<h1>Invoice for Acme Corp</h1>
<p>Amount due: $1,500.00</p>
```

## Section Overview

<div class="grid cards" markdown>

-   :material-variable:{ .lg .middle } **Variables & Data**

    ---

    Learn the basics of placeholders and JSON data

    [:octicons-arrow-right-24: Variables](variables.md)

-   :material-database:{ .lg .middle } **Data Binding**

    ---

    JSON patterns, nested access, array handling

    [:octicons-arrow-right-24: Data Binding](data-binding.md)

-   :material-language-html5:{ .lg .middle } **Liquid Reference**

    ---

    Complete Liquid syntax quick reference

    [:octicons-arrow-right-24: Liquid Reference](liquid-reference.md)

-   :material-filter:{ .lg .middle } **Filters & Tags**

    ---

    TemplateTo's custom filters and tags

    [:octicons-arrow-right-24: Filters & Tags](filters-tags.md)

-   :material-function:{ .lg .middle } **Functions**

    ---

    Advanced functions for complex templates

    [:octicons-arrow-right-24: Functions](functions/index.md)

-   :material-file-document-multiple:{ .lg .middle } **Page Placeholders**

    ---

    Page numbers, dates, and totals

    [:octicons-arrow-right-24: Page Placeholders](page-placeholders.md)

-   :material-translate:{ .lg .middle } **Localization**

    ---

    Culture settings and regional formatting

    [:octicons-arrow-right-24: Localization](localization.md)

-   :material-bug:{ .lg .middle } **Debugging**

    ---

    Troubleshoot template issues

    [:octicons-arrow-right-24: Debugging](debugging.md)

-   :material-palette:{ .lg .middle } **Themes**

    ---

    Share CSS styling across templates

    [:octicons-arrow-right-24: Themes](themes.md)

-   :material-chef-hat:{ .lg .middle } **Recipes**

    ---

    Step-by-step tutorials

    [:octicons-arrow-right-24: Recipes](recipes/index.md)

</div>

## External Resources

- [Liquid Documentation](https://shopify.github.io/liquid/) - Official Liquid templating guide
- [.NET Number Formats](https://learn.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings) - Format string reference
- [Date Format Strings](https://strftime.net/) - Interactive date format builder
