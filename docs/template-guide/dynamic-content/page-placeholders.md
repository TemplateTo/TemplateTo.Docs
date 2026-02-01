# Page Placeholders

Page placeholders insert dynamic page information into PDF headers and footers. These are processed during PDF generation, not during Liquid templating.

## Available Placeholders

| Placeholder | Description | Example Output |
|-------------|-------------|----------------|
| `{{tt_pageNumber}}` | Current page number | 1, 2, 3... |
| `{{tt_totalPages}}` | Total number of pages | 5 |
| `{{tt_date}}` | Current date | (browser locale date) |

## Usage

Page placeholders work **only in PDF headers and footers**. They're replaced at PDF render time by the browser engine.

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

To hide page numbers on the first page, use CSS:

```html
<style>
    @page:first {
        @bottom-center { content: none; }
    }
</style>
<div style="text-align: center;">
    Page {{tt_pageNumber}}
</div>
```

## Limitations

- Page placeholders only work in PDF output (not images or text)
- They must be in the header or footer section of your template settings
- They cannot be used with Liquid logic (e.g., `{% if tt_pageNumber == 1 %}` won't work)
- The `{{tt_date}}` format depends on the browser's locale settings

## See Also

- [PDF Generation](../output-formats/pdf.md) - PDF-specific settings
- [Template Settings](../creating/settings.md) - Configuring headers and footers
