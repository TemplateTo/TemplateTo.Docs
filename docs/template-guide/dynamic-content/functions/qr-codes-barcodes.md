# QR Codes & Barcodes

Generate QR codes and barcodes directly in your templates using the `qrCode()` and `barcode()` functions.

## qrCode()

Generate QR codes that can encode URLs, text, or any string data.

### Syntax

```liquid
{{ qrCode(content, foreground, background, margin, width, height) }}
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `content` | string | (required) | Data to encode (URL, text, etc.) |
| `foreground` | string | `#000000` | Foreground color (hex) |
| `background` | string | `#ffffff` | Background color (hex) |
| `margin` | number | `0` | Margin around code (pixels) |
| `width` | number | `136` | Width (pixels) |
| `height` | number | `136` | Height (pixels) |

### Basic Example

```liquid
{{ qrCode("https://example.com") }}
```

### With Styling

```liquid
{{ qrCode("https://example.com", "#1a1a1a", "#f5f5f5", 10, 200, 200) }}
```

### Using Variables

```liquid
{{ qrCode(order.trackingUrl) }}

{{ qrCode(invoice.paymentLink, "#000", "#fff", 5, 150, 150) }}
```

### Dynamic Content

You can use Liquid variables inside the content:

```liquid
{{ qrCode("https://myapp.com/order/{{order.id}}") }}
```

### Output

The `qrCode()` function outputs an inline SVG wrapped in a span:

```html
<span style="padding: 10px; background: #FFF; display: inline-block;">
    <svg>...</svg>
</span>
```

## barcode()

Generate various barcode formats for product labels, shipping, and inventory.

### Syntax

```liquid
{{ barcode(content, format, foreground, background, fontSize, margin, width, height, noText) }}
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `content` | string | (required) | Data to encode |
| `format` | string | (required) | Barcode format (see below) |
| `foreground` | string | `#000000` | Foreground color (hex) |
| `background` | string | `#ffffff` | Background color (hex) |
| `fontSize` | number | `10` | Text font size |
| `margin` | number | `0` | Margin around code (pixels) |
| `width` | number | `136` | Width (pixels) |
| `height` | number | `136` | Height (pixels) |
| `noText` | boolean | `false` | Hide the text below barcode |

### Supported Formats

| Format | Description | Use Case |
|--------|-------------|----------|
| `CODE128` | High-density alphanumeric | General purpose, shipping |
| `CODE39` | Alphanumeric | Industrial, government |
| `EAN13` | 13-digit numeric | Retail products (Europe) |
| `EAN8` | 8-digit numeric | Small retail products |
| `UPCA` | 12-digit numeric | Retail products (US) |
| `UPCE` | 6-digit numeric | Small retail products (US) |
| `ITF` | Interleaved 2 of 5 | Shipping, warehouse |
| `QR_CODE` | QR Code | Use `qrCode()` instead |

### Basic Example

```liquid
{{ barcode("ABC123456", "CODE128") }}
```

### Product Labels

```liquid
{{ barcode(product.sku, "CODE128", "#000", "#fff", 12, 5, 200, 80) }}
```

### Shipping Labels

```liquid
{{ barcode(shipment.trackingNumber, "CODE128", "#000", "#fff", 10, 10, 300, 100) }}
```

### Without Text

```liquid
{{ barcode(product.upc, "UPCA", "#000", "#fff", 10, 0, 200, 60, true) }}
```

### Using Variables

```liquid
{{ barcode(order.id, "CODE128") }}
{{ barcode(product.ean, "EAN13", "#000", "#fff", 8, 5, 150, 70) }}
```

## Real-World Examples

### Invoice with Payment QR Code

```html
<div class="invoice">
    <h1>Invoice #{{ invoice.number }}</h1>

    <div class="payment-section">
        <h3>Scan to Pay</h3>
        {{ qrCode(invoice.paymentUrl, "#000", "#fff", 10, 150, 150) }}
        <p class="small">Or visit: {{ invoice.paymentUrl }}</p>
    </div>
</div>
```

### Shipping Label

```html
<div class="shipping-label">
    <div class="address">
        <strong>{{ shipment.recipient.name }}</strong><br>
        {{ shipment.recipient.address }}<br>
        {{ shipment.recipient.city }}, {{ shipment.recipient.zip }}
    </div>

    <div class="barcode">
        {{ barcode(shipment.trackingNumber, "CODE128", "#000", "#fff", 10, 5, 300, 80) }}
    </div>

    <div class="qr">
        {{ qrCode(shipment.trackingUrl, "#000", "#fff", 5, 80, 80) }}
    </div>
</div>
```

### Product Label

```html
<div class="product-label">
    <h4>{{ product.name }}</h4>
    <p class="price">{{ product.price | format_number: "C" }}</p>

    {{ barcode(product.sku, "CODE128", "#000", "#fff", 8, 0, 150, 50, true) }}
    <p class="sku">{{ product.sku }}</p>
</div>
```

### Conditional QR Code

```liquid
{% if order.hasDigitalTicket %}
<div class="ticket">
    <h3>Your Digital Ticket</h3>
    {{ qrCode(order.ticketUrl, "#000", "#fff", 10, 200, 200) }}
    <p>Present this code at entry</p>
</div>
{% endif %}
```

## Styling Tips

### Centering Codes

```html
<div style="text-align: center;">
    {{ qrCode(url, "#000", "#fff", 10, 150, 150) }}
</div>
```

### Adding Borders

```html
<div style="border: 1px solid #ccc; padding: 10px; display: inline-block;">
    {{ barcode(sku, "CODE128") }}
</div>
```

### Multiple Codes in a Row

```html
<div style="display: flex; gap: 20px;">
    <div>
        {{ qrCode(url1, "#000", "#fff", 5, 100, 100) }}
        <p>Code 1</p>
    </div>
    <div>
        {{ qrCode(url2, "#000", "#fff", 5, 100, 100) }}
        <p>Code 2</p>
    </div>
</div>
```

## Troubleshooting

### Empty/Blank Output

- Check that the content parameter is not empty or null
- Verify variable paths are correct

### Barcode Not Scanning

- Ensure sufficient contrast between foreground and background
- Check that the barcode format matches your data type
- Increase width/height if the barcode is too small
- Some formats have specific data requirements (e.g., EAN-13 needs exactly 13 digits)

### QR Code Too Small

Increase width and height parameters:

```liquid
{{ qrCode(url, "#000", "#fff", 10, 200, 200) }}
```

### Color Not Applying

Ensure colors are valid hex codes with `#`:

```liquid
<!-- Wrong -->
{{ qrCode(url, "000000", "ffffff") }}

<!-- Right -->
{{ qrCode(url, "#000000", "#ffffff") }}
```

## See Also

- [Functions Overview](index.md) - All available functions
- [Variables](../variables.md) - Using dynamic data
- [Recipes: Shipping Labels](../recipes/iteration-patterns.md) - Complete label examples
