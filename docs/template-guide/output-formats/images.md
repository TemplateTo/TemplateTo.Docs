# Image Generation

Render your templates as PNG or JPEG images. This is useful for social media graphics, thumbnails, email headers, and any web-displayable content.

## Overview

Image output is ideal for:

- Social media graphics (Instagram, Twitter, LinkedIn)
- Open Graph images for link previews
- Email headers and signatures
- Thumbnails and previews
- Certificates and badges
- Profile pictures and avatars

## Image Formats

### PNG

- **Transparency support** - Create images with transparent backgrounds
- **Lossless compression** - No quality loss
- **Best for** - Graphics, logos, images with text, anything requiring transparency

### JPEG

- **Smaller file sizes** - Better compression for photographs
- **Adjustable quality** - Trade quality for size
- **Best for** - Photographs, images where transparency isn't needed

## Image Settings

When rendering images via the API, you can control these settings:

| Setting | Default | Description |
|---------|---------|-------------|
| `format` | `png` | Output format: `png` or `jpeg` |
| `width` | *from template* | Image width in pixels |
| `height` | *auto* | Image height in pixels (only when `fullPage=false`) |
| `quality` | `90` | JPEG quality (0-100). Higher = better quality, larger file |
| `fullPage` | `true` | Capture entire scrollable content |
| `scale` | `1` | Device scale factor (use `2` for retina/high-DPI) |

### Width and Height

By default, image width is derived from your template's page size setting. You can override this with the `width` parameter.

When `fullPage=true` (default), height is determined automatically based on content. Set `fullPage=false` to use a fixed height.

### Scale Factor

The `scale` parameter controls pixel density:

- `scale=1` - Standard resolution (1x)
- `scale=2` - Retina/high-DPI resolution (2x)

A 1200x630 image at `scale=2` produces a 2400x1260 pixel image that displays sharply on retina screens.

### Full Page vs Fixed Height

**Full Page (`fullPage=true`):**

- Captures all content regardless of length
- Height adjusts automatically
- Use for templates with variable content

**Fixed Height (`fullPage=false`):**

- Captures exactly the viewport size
- Set specific `height` parameter
- Use for fixed-size outputs (social media dimensions)

## Clip Regions

Extract specific portions of your rendered template using clip regions.

### Single Clip

Capture a rectangular region from the rendered content:

```json
{
  "clipX": 0,
  "clipY": 0,
  "clipWidth": 800,
  "clipHeight": 400
}
```

| Parameter | Description |
|-----------|-------------|
| `clipX` | X coordinate (pixels from left edge) |
| `clipY` | Y coordinate (pixels from top edge) |
| `clipWidth` | Width of region to capture |
| `clipHeight` | Height of region to capture |

### Multiple Clips

Extract multiple regions from a single render using the clips endpoint. This is efficient when you need several variations from the same template.

```json
{
  "data": {
    "customerName": "Acme Corp"
  },
  "clips": [
    { "name": "header", "x": 0, "y": 0, "width": 1200, "height": 300 },
    { "name": "og-image", "x": 0, "y": 0, "width": 1200, "height": 630 },
    { "name": "thumbnail", "x": 0, "y": 0, "width": 400, "height": 400 }
  ]
}
```

**Use cases:**

- Generate multiple social media sizes from one design
- Create both full image and thumbnail in one request
- Extract header, body, and footer sections separately

### Circular Masks

Create circular (or elliptical) images with transparent backgrounds - perfect for avatars and profile pictures.

```json
{
  "clipX": 0,
  "clipY": 0,
  "clipWidth": 200,
  "clipHeight": 200,
  "circular": true
}
```

When `circular=true`:

- The clip region is masked to a circle (or ellipse if width ≠ height)
- Output is automatically PNG to support transparency
- Background outside the circle is transparent

**Example use cases:**

- User avatars
- Profile pictures
- Round badges or icons

## API Examples

### Basic Image Generation

```bash
# Generate PNG image (default)
curl -X POST "https://api.templateto.com/render/image/tpl_abc123" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"customerName": "Acme Corp"}' \
  -o output.png
```

### JPEG with Custom Settings

```bash
# Generate JPEG with specific width and quality
curl -X POST "https://api.templateto.com/render/image/tpl_abc123?format=jpeg&width=1200&quality=85" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"title": "Summer Sale"}' \
  -o output.jpg
```

### Retina Image

```bash
# Generate 2x resolution for retina displays
curl -X POST "https://api.templateto.com/render/image/tpl_abc123?scale=2&width=1200" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{}' \
  -o output@2x.png
```

### Circular Avatar

```bash
# Generate circular profile picture
curl -X POST "https://api.templateto.com/render/image/tpl_profile?clipX=0&clipY=0&clipWidth=200&clipHeight=200&circular=true" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"userName": "John Doe"}' \
  -o avatar.png
```

### Multiple Clips

```javascript
const response = await fetch(
  'https://api.templateto.com/render/image/tpl_abc123/clips',
  {
    method: 'POST',
    headers: {
      'X-Api-Key': 'your-api-key',
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      data: { title: 'Product Launch' },
      clips: [
        { name: 'instagram', x: 0, y: 0, width: 1080, height: 1080 },
        { name: 'twitter', x: 0, y: 0, width: 1200, height: 675 },
        { name: 'og', x: 0, y: 0, width: 1200, height: 630 }
      ]
    })
  }
);

const result = await response.json();
// result.clips = [{ name, data (base64), format, width, height }, ...]
```

### Image from Raw HTML

```bash
curl -X POST "https://api.templateto.com/render/image/fromhtml" \
  -H "X-Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{
    "base64HtmlString": "PGh0bWw+PGJvZHk+SGVsbG8gV29ybGQ8L2JvZHk+PC9odG1sPg==",
    "format": "png",
    "width": 800
  }' \
  -o output.png
```

## Common Image Sizes

Here are recommended dimensions for common use cases:

| Platform/Use | Width | Height | Aspect Ratio |
|--------------|-------|--------|--------------|
| Open Graph (og:image) | 1200 | 630 | 1.91:1 |
| Twitter Card | 1200 | 675 | 16:9 |
| Instagram Square | 1080 | 1080 | 1:1 |
| Instagram Portrait | 1080 | 1350 | 4:5 |
| LinkedIn | 1200 | 627 | 1.91:1 |
| Email Header | 600 | 200 | 3:1 |

## Best Practices

1. **Choose the right format**
   - Use PNG for graphics, text, or transparency
   - Use JPEG for photographs or when file size matters

2. **Optimize for platform**
   - Match dimensions to platform requirements
   - Use appropriate scale factor for the target device

3. **Use clips efficiently**
   - Generate multiple sizes in one request
   - Design templates with clip regions in mind

4. **Consider file size**
   - Lower JPEG quality (70-85) for web thumbnails
   - Higher quality (90-100) for print or download

5. **Test transparency**
   - Verify PNG transparency displays correctly
   - Check how circular masks look on different backgrounds

For complete API documentation, see the [REST API Reference](../../developer-guide/rest-api.md).
