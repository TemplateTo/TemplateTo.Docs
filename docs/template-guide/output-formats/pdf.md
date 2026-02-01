# PDF Generation

TemplateTo generates high-quality PDF documents from your templates. This guide covers PDF-specific settings and features.

## Overview

PDF output is ideal for:

- Printable documents (invoices, contracts, reports)
- Multi-page content with page breaks
- Documents requiring headers and footers
- Password-protected files
- Official documents requiring consistent formatting

## PDF Settings

Access PDF settings through the Template Settings panel in the editor.

![Settings button](../../images/a5e1ca4966568a3363fb78f94b225e2be86c05eb96c779b9bbfc3f2747105c36.png)

### Paper Size

Choose from standard sizes or define custom dimensions:

![Paper size options](../../images/45f42f63e96e8dde37e2d79fa1272e579cad886472a72a21ee74483d6d6f3d38.png)

**Standard sizes:**

- A4 (210mm x 297mm) - Default
- Letter (8.5" x 11")
- Legal (8.5" x 14")
- A3, A5, and more

**Custom size:** Select "Custom" and enter dimensions in millimeters.

### Orientation

- **Portrait** (default) - Taller than wide
- **Landscape** - Wider than tall

### Print Background

Enable to include background colors and images in the PDF. This is enabled by default.

### Margins

![Margins settings](../../images/9b12878689f112de6c9b3f90b6c44fba1c360b893ec8eb6e4288fa2af23186e2.png)

Set margins for top, bottom, left, and right edges.

!!! note
    Headers and footers render within the margins. Ensure adequate margin space when using headers/footers.

## Headers and Footers

Add consistent headers and footers to every page of your document.

### Enabling Headers/Footers

1. Open Template Settings
2. Toggle "Display Header / Footer"
3. Select content blocks for header and/or footer

![Enable Headers & footers](../../images/6a7ab088c0dcfa1d77790f8f013f2357ebd966a9d70baa807f7b4aa9353e63a1.png)

### Using Content Blocks

Headers and footers use [Content Blocks](../creating/elements.md#content-blocks) - reusable pieces of content you can share across templates.

![Select header / footer content blocks](../../images/3260c022f378f82841120f3b08773ee79c2d0eefeec4923e47403978eb3b217e.png)

Create content blocks at: [Content Block Manager](https://app.templateto.com/templates/blocks)

### Page Numbers

Use Liquid variables in your header/footer content blocks:

- `{{page}}` - Current page number
- `{{pages}}` - Total page count

Example footer: `Page {{page}} of {{pages}}`

## Cover Page

Apply different settings to the first page of your document.

![Cover page options](../../images/f29bb89daab848ad0bd988bf2eb4c8edde1a83e6725df445c6102000943dd92f.png)

Cover page options let you:

- Use different margins for page 1
- Hide headers/footers on the cover
- Apply different orientation

## Password Protection

Secure your PDFs with password protection and permission controls.

### Enabling Protection

1. Open Template Settings
2. Toggle "Password Protection"
3. Configure options

![Enabling Password protection](../../images/5e19c35586294157ce352d07919a607f644d81596004fd5770d02ad15472b0c9.png)

### Protection Options

![Password protection options](../../images/8e3c20b1eeaa4e9839e17aaf958cb221f0bb6d9decc57302021ffe46096adc67.png)

| Option | Description |
|--------|-------------|
| Password | Password to open the document. Supports variables like `{{customerPassword}}` |
| Permit Print | Allow printing |
| Permit Modify | Allow editing document content |
| Permit Extract | Allow copying text and images |
| Permit Annotations | Allow adding comments and annotations |

!!! tip "Dynamic Passwords"
    The password field accepts template variables. Use `{{password}}` and pass a unique password in your API request for per-document encryption.

## Page Breaks

Control where new pages begin in your document.

### Page Break Element

Add a Page Break element to force content onto a new page:

![Example page with a Page Break](../../images/7217146102004a8a8a2287bdc448ee3b07a6f595a206779169d9f4a5facf0fd8.png)

1. The Page Break element fills remaining space on the current page
2. Content after the break appears on the next page

### Automatic Page Breaks

TemplateTo calculates estimated page breaks automatically. The editor shows dotted lines indicating where breaks will occur.

To disable this calculation (useful for templates with complex layouts), toggle "Disable PageBreak Calculation" in Template Settings.

## API Integration

### Generate PDF from Template

```bash
curl -X POST "https://api.templateto.com/render/pdf/{templateId}" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"customerName": "Acme Corp", "total": 150.00}'
```

### Generate PDF from Raw HTML

```bash
curl -X POST "https://api.templateto.com/render/pdf/fromhtml" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"base64HtmlString": "PGh0bWw+PGJvZHk+SGVsbG8gV29ybGQ8L2JvZHk+PC9odG1sPg=="}'
```

!!! tip "Base64 Encoding"
    Raw HTML must be Base64 encoded. In JavaScript: `btoa(htmlString)`. In Python: `base64.b64encode(html_string.encode()).decode()`.

### Response Types

- **File download** - `POST /render/pdf/{templateId}` returns the PDF directly
- **Base64 encoded** - `POST /render/pdf/{templateId}/as-byte-array` returns a Base64 string

For complete API documentation, see the [REST API Reference](../../developer-guide/rest-api.md).

## Best Practices

1. **Test with realistic data** - Use actual content lengths to verify page breaks
2. **Set appropriate margins** - Leave room for headers/footers and binding
3. **Use content blocks** - Share headers/footers across related templates
4. **Consider print settings** - Enable "Print Background" if you use background colors
5. **Validate passwords** - Test password-protected PDFs open correctly
