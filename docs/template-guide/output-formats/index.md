# Output Formats

TemplateTo can generate documents in multiple formats. Choose the right format for your use case.

## Available Formats

<div class="grid cards" markdown>

- :material-file-pdf-box:{ .lg .middle } **PDF Generation**

    ---

    Multi-page documents with headers, footers, and password protection

    [:octicons-arrow-right-24: PDF Guide](pdf.md)

- :material-image:{ .lg .middle } **Image Generation**

    ---

    PNG and JPEG images with clips, masks, and retina support

    [:octicons-arrow-right-24: Image Guide](images.md)

- :material-text:{ .lg .middle } **Plain Text**

    ---

    TXT output for emails, SMS, and data exports

    [:octicons-arrow-right-24: TXT Guide](txt.md)

</div>

## Format Comparison

| Feature | PDF | PNG | JPEG | TXT |
|---------|-----|-----|------|-----|
| Multi-page | Yes | No | No | Yes |
| Formatting | Full | Full | Full | None |
| Transparency | No | Yes | No | N/A |
| File size | Medium | Large | Small | Tiny |
| Password protection | Yes | No | No | No |
| Headers/footers | Yes | No | No | No |
| Printable | Yes | Yes | Yes | Yes |
| Web display | Embed | Native | Native | Pre-formatted |

## When to Use Each Format

### PDF

Best for documents that need to be:

- Printed or downloaded
- Multi-page with consistent formatting
- Password protected
- Archived with exact layout preservation

**Examples:** Invoices, contracts, reports, certificates

### PNG

Best for images that need:

- Transparency (logos, overlays)
- Lossless quality
- Sharp text and graphics

**Examples:** Social media graphics, logos, diagrams, Open Graph images

### JPEG

Best for images that need:

- Smaller file sizes
- Photographic content
- Fast web loading

**Examples:** Thumbnails, email headers, photo-based graphics

### TXT

Best for content that needs:

- Maximum compatibility
- No formatting
- Small file size
- Easy parsing

**Examples:** Email body content, SMS messages, data exports, notifications

## API Endpoints

All formats are available through both sync and async APIs:

| Format | Sync Endpoint | Async Endpoint |
|--------|---------------|----------------|
| PDF | `POST /render/pdf/{templateId}` | `POST /render/async/pdf/{templateId}` |
| Image | `POST /render/image/{templateId}` | `POST /render/async/image/{templateId}` |
| TXT | `POST /render/txt/{templateId}` | `POST /render/async/txt/{templateId}` |

See the [REST API Reference](../../developer-guide/rest-api.md) for complete documentation.
