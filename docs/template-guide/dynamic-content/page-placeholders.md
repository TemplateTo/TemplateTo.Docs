# Page Placeholders

Page placeholders insert dynamic page information into PDF headers and footers. These are processed during PDF generation, not during Liquid templating.

## Available Placeholders

| Placeholder | Description | Example Output |
|-------------|-------------|----------------|
| `{{tt_pageNumber}}` | Current page number | 1, 2, 3... |
| `{{tt_totalPages}}` | Total number of pages | 5 |
| `{{tt_date}}` | Current date | (browser locale date) |

!!! tip "Need numbering that restarts, or page numbers in the body?"
    Turn on [Advanced Layout Control](../advanced-layout-control.md). It adds `{{tt_documentPages}}` and `{{tt_overallPageNumber}}` and makes page placeholders work in the body. You can [start at a chosen number](../advanced-layout-control.md#start-numbering-at-a-chosen-number) or [supply it as template data](../advanced-layout-control.md#read-the-starting-number-from-template-data).

## Usage

With standard PDF layout, page placeholders go in the header or footer. With Advanced Layout Control, they also work in the body. In both cases, the browser fills them in during PDF generation.

### Adding to Header/Footer

In your template settings, add placeholders to your header or footer HTML:

**Footer example:**
```html
<div style="text-align: center; font-size: 10px; color: #666;">
    Page {{tt_pageNumber}} of {{tt_totalPages}}
</div>
```

**Header example:**
```html
<div style="display: flex; justify-content: space-between; font-size: 9px;">
    <span>{{tt_date}}</span>
    <span>Confidential Document</span>
    <span>Page {{tt_pageNumber}}</span>
</div>
```

## How It Works

Page placeholders are converted to special CSS class spans that the PDF renderer (Chromium) populates:

| Placeholder | Becomes |
|-------------|---------|
| `{{tt_date}}` | `<span class="date"></span>` |
| `{{tt_pageNumber}}` | `<span class="pageNumber"></span>` |
| `{{tt_totalPages}}` | `<span class="totalPages"></span>` |

The browser engine fills these spans with the actual values during PDF generation.

## Complete Footer Example

```html
<div style="
    width: 100%;
    font-size: 9px;
    color: #888;
    padding: 10px 20px;
    display: flex;
    justify-content: space-between;
    border-top: 1px solid #eee;
">
    <span>Generated: {{tt_date}}</span>
    <span>{{companyName}}</span>
    <span>Page {{tt_pageNumber}} of {{tt_totalPages}}</span>
</div>
```

!!! note "Mixing with Liquid Variables"
    You can use both page placeholders and Liquid variables in headers/footers. In the example above, `{{companyName}}` is a Liquid variable from your JSON data, while `{{tt_pageNumber}}` is a page placeholder.

## Styling Tips

### Centered Page Numbers

```html
<div style="text-align: center; font-size: 10px;">
    - {{tt_pageNumber}} -
</div>
```

### Right-Aligned with Total

```html
<div style="text-align: right; font-size: 9px; padding-right: 20px;">
    {{tt_pageNumber}} / {{tt_totalPages}}
</div>
```

### Conditional First Page

Use the template's **Cover page** setting to give the first page its own footer. With Advanced Layout Control, you can also [hide the first page's header and footer in CSS or start the count after the cover](../advanced-layout-control.md#cover-page-without-a-header-or-footer).

## Limitations

- Page placeholders only work in PDF output (not images or text)
- They cannot be used with Liquid logic (e.g., `{% if tt_pageNumber == 1 %}` won't work)
- The `{{tt_date}}` format depends on the browser's locale settings

## See Also

- [Advanced Layout Control](../advanced-layout-control.md) - restarting page numbers, per-document totals and CSS page rules
- [PDF Generation](../output-formats/pdf.md) - PDF-specific settings
- [Template Settings](../creating/settings.md) - Configuring headers and footers
