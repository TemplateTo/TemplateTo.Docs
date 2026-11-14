# Advanced Layout Control

Advanced Layout Control gives you control over page numbering, headers and footers that the standard PDF output doesn't have. With it on you can:

- restart page numbers part-way through a PDF, so one PDF can hold several documents that each start at page 1
- show a page total for each of those documents, as well as for the whole PDF
- use page numbers in the body of the document, not only in the header and footer
- give each document in the PDF its own header and footer
- set headers, footers and numbering with standard CSS for printed pages (`@page` rules, page counters, running titles)

It is only used for PDF output.

## Turning it on

1. Open the template and choose **Settings**.
2. Set **Render Engine** to **Version 4**.
3. Turn on **Advanced Layout Control** and save.

The toggle is greyed out on any other render engine, because only Version 4 supports it. Choosing another engine turns it off.

Nothing changes in how you generate documents. The same API calls, integrations and preview work as before; the template's PDF is laid out differently.

!!! note "Check your template after turning it on"
    Your existing header and footer content blocks, margins, page size and cover page settings all carry over. Page breaks can still land in slightly different places, so look through the PDF preview before you publish.

## Page placeholders

With Advanced Layout Control on, page placeholders work in the header, the footer **and the body** of the template.

| Placeholder | What it shows |
|-------------|---------------|
| `{{tt_pageNumber}}` | The current page number. Restarts wherever you restart numbering. |
| `{{tt_documentPages}}` | The number of pages in the current document, which is the run of pages since numbering last restarted. |
| `{{tt_overallPageNumber}}` | The page's position in the whole PDF, whatever the restarts. |
| `{{tt_totalPages}}` | The number of pages in the whole PDF. |
| `{{tt_date}}` | Today's date, in the template's culture. |

`{{tt_documentPages}}` and `{{tt_overallPageNumber}}` are only filled in when Advanced Layout Control is on.

A footer that shows both counts:

```html
<div style="display: flex; justify-content: space-between; font-size: 9px;">
    <span>Page {{tt_pageNumber}} of {{tt_documentPages}}</span>
    <span>Sheet {{tt_overallPageNumber}} of {{tt_totalPages}}</span>
</div>
```

## Several documents in one PDF

Put each document in an element with the class `tt-document`. Each one starts on a new page and its pages are numbered from 1.

```html
{% for invoice in invoices %}
<section class="tt-document">
    <h1>Invoice {{ invoice.number }}</h1>
    <p>{{ invoice.customer }}</p>
    <!-- the rest of the invoice -->
</section>
{% endfor %}
```

With the footer above, a three-page invoice followed by a two-page invoice is numbered "Page 1 of 3", "Page 2 of 3", "Page 3 of 3", then "Page 1 of 2", "Page 2 of 2".

### A header and footer for each document

Inside a `tt-document`, an element with the class `tt-document-header` or `tt-document-footer` becomes the header or footer from that page on. It replaces the template's own header or footer until the next document sets another one.

```html
{% for invoice in invoices %}
<section class="tt-document">
    <div class="tt-document-header">
        <strong>{{ company }}</strong> Invoice {{ invoice.number }}
    </div>
    <div class="tt-document-footer">
        {{ invoice.customer }}, page {{tt_pageNumber}} of {{tt_documentPages}}
    </div>
    <h1>Invoice {{ invoice.number }}</h1>
    <!-- the rest of the invoice -->
</section>
{% endfor %}
```

Headers and footers are drawn in the page margins, as they are without Advanced Layout Control, so the template's margins need to leave room for them.

## Recipes

Add the CSS in these recipes to the template's CSS.

### Cover page without a header or footer

Use the template's **Cover page** setting, which gives the first page its own margins and its own header and footer (or none). Or do it in CSS:

```css
@page :first {
    @top-center { content: none; }
    @bottom-center { content: none; }
}
```

The cover still counts as page 1.

### Cover page that isn't counted

Restart the count at 0 on the cover, so the page after it is page 1. Use `{{tt_documentPages}}` for the total, which then leaves the cover out as well.

```css
.cover { counter-reset: page 0; }
@page :first {
    @top-center { content: none; }
    @bottom-center { content: none; }
}
```

```html
<div class="cover"><h1>Method Statement</h1></div>
<div class="page-break"></div>
<h1>Scope</h1>
```

### Start numbering at a chosen number

```css
.appendix { counter-reset: page 12; }
```

The page where `.appendix` starts is page 12, the next is 13, and so on.

### Roman numerals for the front matter

Make the front matter and the main part two documents. In the front matter's footer, show the page counter in roman numerals with a small piece of CSS:

```css
.roman-page::before { content: counter(page, lower-roman); }
```

```html
<section class="tt-document">
    <div class="tt-document-footer">Front matter <span class="roman-page"></span></div>
    <h1>Preface</h1>
</section>
<section class="tt-document">
    <div class="tt-document-footer">Page {{tt_pageNumber}}</div>
    <h1>Scope</h1>
</section>
```

### Sheet numbers with a prefix

Put the prefix in each document's footer: `Sheet A-{{tt_pageNumber}}` in appendix A, `Sheet B-{{tt_pageNumber}}` in appendix B. Each appendix is a `tt-document`, so each starts again at 1.

### Contents page with page numbers

Link each contents entry to its heading, and let CSS look up the page the heading is on:

```css
.toc a::after { content: " .... p. " target-counter(attr(href url), page); }
```

```html
<ul class="toc">
    {% for chapter in chapters %}
    <li><a href="#chapter-{{ forloop.index }}">{{ chapter }}</a></li>
    {% endfor %}
</ul>
{% for chapter in chapters %}
<div class="chapter" id="chapter-{{ forloop.index }}">
    <h1>{{ chapter }}</h1>
</div>
{% endfor %}
```

### The current chapter's title in the header

```css
h1 { string-set: chapter content(text); }
@page { @top-right { content: "Chapter: " string(chapter); } }
```

### Different footers on left and right pages

```css
@page :left { @bottom-left { content: "Page " counter(page); } }
@page :right { @bottom-right { content: "Page " counter(page); } }
@page :blank { @bottom-left { content: none; } @bottom-right { content: none; } }
.chapter + .chapter { break-before: right; }
```

`break-before: right` starts each chapter on a right-hand page, adding a blank page when it needs to.

### Table header row on every page

Nothing to add. When a table with a `<thead>` runs over several pages, its header row is repeated at the top of each page.

## Limits

- **Render engine Version 4 only.** The other render engines ignore the setting.
- **One page size per PDF.** A section with its own page size, such as one landscape page in a portrait document, prints at the template's page size.
- **Margin text on a named page is added to the template's footer**, not shown instead of it. To change the header or footer for part of a PDF, make that part a `tt-document` with its own `tt-document-header` or `tt-document-footer`.
- **Very long PDFs take longer.** A few hundred pages is fine. For packs of several hundred pages or more, render the documents separately.
- **Stylesheets linked from other sites** are applied by the browser but can't be read for page rules. Put `@page` rules and counters in the template's own CSS.

## When a document can't be laid out

If the pages can't be laid out, the render fails straight away with an error instead of producing a PDF with the wrong layout. The API returns status `422` with a message that starts "Advanced layout control could not lay out this document's pages", followed by the reason. An async render job ends with the status `Failed` and the same message, and is not retried.

One cause we have seen is an empty element placed directly after a table that runs over a page. If a template with long tables fails, look for that first: remove the empty element or put some content in it.

## See Also

- [Page Placeholders](dynamic-content/page-placeholders.md) - placeholders without Advanced Layout Control
- [Template Settings](creating/settings.md) - render engine, margins and the cover page
- [PDF Generation](output-formats/pdf.md) - PDF-specific settings
