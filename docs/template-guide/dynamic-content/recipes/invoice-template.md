# Tutorial: Building an Invoice Template

This tutorial walks through creating a complete invoice template from scratch. You'll learn how to structure your data, display company and customer information, iterate over line items, and calculate totals.

## What We're Building

A professional invoice with:
- Company header
- Customer billing details
- Line items table
- Subtotal, tax, and total
- Payment information
- Page numbers

## Step 1: Define Your Data Structure

Start by planning your JSON data. Here's a typical invoice structure:

```json
{
  "company": {
    "name": "Acme Solutions Ltd",
    "address": "123 Business Park",
    "city": "London",
    "postcode": "EC1A 1BB",
    "phone": "+44 20 1234 5678",
    "email": "billing@acme.com",
    "vatNumber": "GB123456789"
  },
  "invoice": {
    "number": "INV-2024-001",
    "date": "15/01/24",
    "dueDate": "15/02/24",
    "poNumber": "PO-5678"
  },
  "customer": {
    "name": "Widget Corp",
    "contact": "Jane Smith",
    "address": "456 Client Street",
    "city": "Manchester",
    "postcode": "M1 1AA"
  },
  "items": [
    {
      "description": "Consulting Services",
      "quantity": 10,
      "unit": "hours",
      "unitPrice": 150.00,
      "total": 1500.00
    },
    {
      "description": "Software License",
      "quantity": 1,
      "unit": "license",
      "unitPrice": 499.00,
      "total": 499.00
    },
    {
      "description": "Support Package",
      "quantity": 1,
      "unit": "month",
      "unitPrice": 200.00,
      "total": 200.00
    }
  ],
  "subtotal": 2199.00,
  "taxRate": 20,
  "taxAmount": 439.80,
  "total": 2638.80,
  "notes": "Payment terms: Net 30 days",
  "bankDetails": {
    "name": "Acme Solutions Ltd",
    "bank": "Example Bank",
    "sortCode": "12-34-56",
    "accountNumber": "12345678"
  }
}
```

Paste this into the Template Data panel in the editor.

## Step 2: Create the Header

Start with the company information and invoice details:

```html
<div class="invoice-header">
    <div class="company-info">
        <h1>{{ company.name }}</h1>
        <p>{{ company.address }}</p>
        <p>{{ company.city }}, {{ company.postcode }}</p>
        <p>{{ company.phone }}</p>
        <p>{{ company.email }}</p>
    </div>

    <div class="invoice-details">
        <h2>INVOICE</h2>
        <table>
            <tr>
                <td>Invoice No:</td>
                <td><strong>{{ invoice.number }}</strong></td>
            </tr>
            <tr>
                <td>Date:</td>
                <td>{{ invoice.date | parse_date | date: "%d %B %Y" }}</td>
            </tr>
            <tr>
                <td>Due Date:</td>
                <td>{{ invoice.dueDate | parse_date | date: "%d %B %Y" }}</td>
            </tr>
            {% if invoice.poNumber %}
            <tr>
                <td>PO Number:</td>
                <td>{{ invoice.poNumber }}</td>
            </tr>
            {% endif %}
        </table>
    </div>
</div>
```

Key techniques:
- `{{ invoice.date | parse_date | date: "%d %B %Y" }}` - Parse and format dates
- `{% if invoice.poNumber %}` - Only show PO number if provided

## Step 3: Add Customer Details

```html
<div class="billing-section">
    <div class="bill-to">
        <h3>Bill To:</h3>
        <p><strong>{{ customer.name }}</strong></p>
        {% if customer.contact %}
        <p>Attn: {{ customer.contact }}</p>
        {% endif %}
        <p>{{ customer.address }}</p>
        <p>{{ customer.city }}, {{ customer.postcode }}</p>
    </div>
</div>
```

## Step 4: Build the Line Items Table

This is where iteration comes in:

```html
<table class="items-table">
    <thead>
        <tr>
            <th>Description</th>
            <th>Qty</th>
            <th>Unit</th>
            <th>Unit Price</th>
            <th>Total</th>
        </tr>
    </thead>
    <tbody>
        {% for item in items %}
        <tr>
            <td>{{ item.description }}</td>
            <td>{{ item.quantity }}</td>
            <td>{{ item.unit }}</td>
            <td>{{ item.unitPrice | format_number: "C" }}</td>
            <td>{{ item.total | format_number: "C" }}</td>
        </tr>
        {% endfor %}
    </tbody>
</table>
```

Key techniques:
- `{% for item in items %}` - Loop through line items
- `{{ item.total | format_number: "C" }}` - Format as currency

## Step 5: Add Totals Section

```html
<div class="totals">
    <table>
        <tr>
            <td>Subtotal:</td>
            <td>{{ subtotal | format_number: "C" }}</td>
        </tr>
        <tr>
            <td>VAT ({{ taxRate }}%):</td>
            <td>{{ taxAmount | format_number: "C" }}</td>
        </tr>
        <tr class="grand-total">
            <td><strong>Total Due:</strong></td>
            <td><strong>{{ total | format_number: "C" }}</strong></td>
        </tr>
    </table>
</div>
```

## Step 6: Add Payment Information

```html
<div class="payment-info">
    <h3>Payment Details</h3>
    <p>Please make payment to:</p>
    <table>
        <tr>
            <td>Account Name:</td>
            <td>{{ bankDetails.name }}</td>
        </tr>
        <tr>
            <td>Bank:</td>
            <td>{{ bankDetails.bank }}</td>
        </tr>
        <tr>
            <td>Sort Code:</td>
            <td>{{ bankDetails.sortCode }}</td>
        </tr>
        <tr>
            <td>Account Number:</td>
            <td>{{ bankDetails.accountNumber }}</td>
        </tr>
    </table>

    {% if notes %}
    <p class="notes"><em>{{ notes }}</em></p>
    {% endif %}
</div>
```

## Step 7: Add Footer

```html
<div class="footer">
    <p>{{ company.name }} | VAT No: {{ company.vatNumber }}</p>
</div>
```

## Step 8: Add Styling

Add CSS to make it look professional:

```css
body {
    font-family: Arial, sans-serif;
    font-size: 12px;
    color: #333;
    line-height: 1.4;
}

.invoice-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 30px;
    padding-bottom: 20px;
    border-bottom: 2px solid #333;
}

.company-info h1 {
    margin: 0 0 10px 0;
    color: #2c5282;
}

.company-info p {
    margin: 2px 0;
}

.invoice-details h2 {
    margin: 0 0 15px 0;
    color: #2c5282;
}

.invoice-details table td {
    padding: 3px 10px 3px 0;
}

.billing-section {
    margin-bottom: 30px;
}

.bill-to h3 {
    margin: 0 0 10px 0;
    color: #666;
    font-size: 11px;
    text-transform: uppercase;
}

.items-table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 30px;
}

.items-table th {
    background: #2c5282;
    color: white;
    padding: 10px;
    text-align: left;
}

.items-table td {
    padding: 10px;
    border-bottom: 1px solid #ddd;
}

.items-table th:nth-child(2),
.items-table th:nth-child(4),
.items-table th:nth-child(5),
.items-table td:nth-child(2),
.items-table td:nth-child(4),
.items-table td:nth-child(5) {
    text-align: right;
}

.totals {
    float: right;
    width: 300px;
}

.totals table {
    width: 100%;
}

.totals td {
    padding: 5px;
}

.totals td:last-child {
    text-align: right;
}

.grand-total {
    border-top: 2px solid #333;
    font-size: 14px;
}

.payment-info {
    clear: both;
    margin-top: 40px;
    padding-top: 20px;
    border-top: 1px solid #ddd;
}

.payment-info h3 {
    margin: 0 0 10px 0;
}

.payment-info table td {
    padding: 3px 15px 3px 0;
}

.notes {
    margin-top: 15px;
    color: #666;
}

.footer {
    margin-top: 40px;
    padding-top: 10px;
    border-top: 1px solid #ddd;
    text-align: center;
    color: #666;
    font-size: 10px;
}
```

## Step 9: Add Page Numbers (PDF Footer)

In your template settings, add a PDF footer:

```html
<div style="text-align: center; font-size: 9px; color: #999;">
    Page {{tt_pageNumber}} of {{tt_totalPages}}
</div>
```

## Complete Template

Here's the full HTML template:

```html
<div class="invoice">
    <div class="invoice-header">
        <div class="company-info">
            <h1>{{ company.name }}</h1>
            <p>{{ company.address }}</p>
            <p>{{ company.city }}, {{ company.postcode }}</p>
            <p>{{ company.phone }}</p>
            <p>{{ company.email }}</p>
        </div>

        <div class="invoice-details">
            <h2>INVOICE</h2>
            <table>
                <tr>
                    <td>Invoice No:</td>
                    <td><strong>{{ invoice.number }}</strong></td>
                </tr>
                <tr>
                    <td>Date:</td>
                    <td>{{ invoice.date | parse_date | date: "%d %B %Y" }}</td>
                </tr>
                <tr>
                    <td>Due Date:</td>
                    <td>{{ invoice.dueDate | parse_date | date: "%d %B %Y" }}</td>
                </tr>
                {% if invoice.poNumber %}
                <tr>
                    <td>PO Number:</td>
                    <td>{{ invoice.poNumber }}</td>
                </tr>
                {% endif %}
            </table>
        </div>
    </div>

    <div class="billing-section">
        <div class="bill-to">
            <h3>Bill To:</h3>
            <p><strong>{{ customer.name }}</strong></p>
            {% if customer.contact %}
            <p>Attn: {{ customer.contact }}</p>
            {% endif %}
            <p>{{ customer.address }}</p>
            <p>{{ customer.city }}, {{ customer.postcode }}</p>
        </div>
    </div>

    <table class="items-table">
        <thead>
            <tr>
                <th>Description</th>
                <th>Qty</th>
                <th>Unit</th>
                <th>Unit Price</th>
                <th>Total</th>
            </tr>
        </thead>
        <tbody>
            {% for item in items %}
            <tr>
                <td>{{ item.description }}</td>
                <td>{{ item.quantity }}</td>
                <td>{{ item.unit }}</td>
                <td>{{ item.unitPrice | format_number: "C" }}</td>
                <td>{{ item.total | format_number: "C" }}</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>

    <div class="totals">
        <table>
            <tr>
                <td>Subtotal:</td>
                <td>{{ subtotal | format_number: "C" }}</td>
            </tr>
            <tr>
                <td>VAT ({{ taxRate }}%):</td>
                <td>{{ taxAmount | format_number: "C" }}</td>
            </tr>
            <tr class="grand-total">
                <td><strong>Total Due:</strong></td>
                <td><strong>{{ total | format_number: "C" }}</strong></td>
            </tr>
        </table>
    </div>

    <div class="payment-info">
        <h3>Payment Details</h3>
        <p>Please make payment to:</p>
        <table>
            <tr>
                <td>Account Name:</td>
                <td>{{ bankDetails.name }}</td>
            </tr>
            <tr>
                <td>Bank:</td>
                <td>{{ bankDetails.bank }}</td>
            </tr>
            <tr>
                <td>Sort Code:</td>
                <td>{{ bankDetails.sortCode }}</td>
            </tr>
            <tr>
                <td>Account Number:</td>
                <td>{{ bankDetails.accountNumber }}</td>
            </tr>
        </table>

        {% if notes %}
        <p class="notes"><em>{{ notes }}</em></p>
        {% endif %}
    </div>

    <div class="footer">
        <p>{{ company.name }} | VAT No: {{ company.vatNumber }}</p>
    </div>
</div>
```

## Next Steps

- Try adding a QR code for payment: `{{ qrCode(paymentUrl) }}`
- Add conditional discounts
- Create a credit note variant
- Set up different cultures for international invoices

## See Also

- [Iteration Patterns](iteration-patterns.md) - More table examples
- [Conditional Content](conditional-content.md) - Show/hide sections
- [Localization](../localization.md) - Multi-currency support
