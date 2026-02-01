# Your First Template

This guide walks you through creating your first template in TemplateTo. By the end, you'll have a working invoice template that you can customize and use with the API.

## Step 1: Create a New Template

1. Log in to [TemplateTo](https://app.templateto.com)
2. Navigate to **Templates** in the sidebar
3. Click **+ New Template**
4. Give your template a name (e.g., "My First Invoice")
5. Click **Create**

You'll now see the template editor with a blank canvas.

## Step 2: Add a Layout

Templates work best when you organize content with layout elements.

1. Click **Elements** in the right panel
2. Find the **Layout** section
3. Drag a **1 Column** layout onto the canvas

This creates a container for your content.

## Step 3: Add a Header

Let's add a title to your invoice:

1. From the **Basic** elements, drag a **Header** element into the column
2. Click on the header text to edit it
3. Type "INVOICE"
4. Use the header toolbar to center the text

## Step 4: Add Company Details

Add text for company information:

1. Drag a **Text** element below the header
2. Type your company details:
   ```
   Acme Corporation
   123 Business Street
   City, State 12345
   ```

## Step 5: Add Dynamic Variables

Now let's make the template dynamic by adding variables that will be replaced with real data.

1. Add another **Text** element
2. Type: `Invoice #: {{invoiceNumber}}`
3. Add another line: `Date: {{date}}`
4. Add another line: `Customer: {{customerName}}`

The `{{variableName}}` syntax creates placeholders that will be filled with data when you generate the document.

### Test Your Variables

1. Click **Data** in the editor menu
2. In the Template Data panel, enter test JSON:
   ```json
   {
     "invoiceNumber": "INV-001",
     "date": "2024-01-15",
     "customerName": "John Smith"
   }
   ```
3. Click **Preview** to see your variables replaced with test data

## Step 6: Add a Table for Line Items

Invoices need line items. Let's add a table:

1. Drag a **Table** element onto the canvas
2. Set up the header row with columns: Item, Quantity, Price, Total
3. In the second row, click the row handle (left side) and click **+**
4. Select **Repeating Row**
5. Choose `lineItems` as the array to repeat

In each cell of the repeating row, add:

- Column 1: `{{_.item}}`
- Column 2: `{{_.quantity}}`
- Column 3: `{{_.price}}`
- Column 4: `{{_.total}}`

The underscore `_` represents the current item in the loop.

### Update Your Test Data

Add line items to your test JSON:

```json
{
  "invoiceNumber": "INV-001",
  "date": "2024-01-15",
  "customerName": "John Smith",
  "lineItems": [
    { "item": "Widget", "quantity": 2, "price": "$10.00", "total": "$20.00" },
    { "item": "Gadget", "quantity": 1, "price": "$25.00", "total": "$25.00" }
  ],
  "grandTotal": "$45.00"
}
```

## Step 7: Add a Total

1. Add a **Text** element below the table
2. Type: `Total: {{grandTotal}}`
3. Style it with the Properties panel (make it bold, align right)

## Step 8: Save and Preview

1. Click **Save** in the top bar
2. Click **Preview** to open the preview in a new tab
3. Your invoice should display with all test data filled in

## Step 9: Generate Your First Document

Now let's generate a real PDF:

1. Click **Generate** in the top bar (or navigate to Code Builder)
2. You'll see options for how to generate the document
3. Enter your data or use the test data
4. Click **Generate PDF**
5. Download your finished invoice!

## Video Tutorial

Want to see this process in action? Watch our video walkthrough:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Aquy1eLA7cM?si=fkynQyw6r7OuNtzY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Next Steps

Now that you've created your first template:

- **[Elements Reference](elements.md)** - Learn about all available elements
- **[Variables & Data](../dynamic-content/variables.md)** - Advanced variable techniques
- **[Template Settings](settings.md)** - Configure page size, headers, footers
- **[PDF Generation](../output-formats/pdf.md)** - PDF-specific features
- **[REST API](../../developer-guide/rest-api.md)** - Generate documents programmatically

## Tips for Better Templates

1. **Plan your layout first** - Sketch what you want before building
2. **Use test data** - Add realistic test data to catch formatting issues
3. **Keep it simple** - Start with basic elements, add complexity later
4. **Use Content Blocks** - Create reusable pieces for headers/footers
5. **Test with edge cases** - Try long text, missing data, many rows
