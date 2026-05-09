# Odoo

Integrate TemplateTo with Odoo 18 to replace the default wkhtmltopdf PDF engine with modern Chromium-based rendering. Your existing QWeb report templates work unchanged — just install the module, enter your API key, and every report uses Chrome headless.

## Overview

The **TemplateTo Reports** module for Odoo overrides `ir.actions.report._render_qweb_pdf()` so that all PDF generation is routed through TemplateTo's API instead of wkhtmltopdf. This gives you:

- **Modern CSS support** — flexbox, grid, media queries, custom fonts, gradients
- **Reliable rendering** — no more blank PDFs, broken headers/footers, or timeout crashes
- **Sync + batch generation** — interactive Print for small batches, background processing for bulk operations
- **Automatic fallback** — if the API is unreachable, Odoo falls back to wkhtmltopdf seamlessly

### Supported Versions

- Odoo 18.0 Community or Enterprise
- Internet connectivity from the Odoo server to `https://api.templateto.com`

## Installation

### From the Odoo Apps Store

The module is published as **[TemplateTo Reports](https://apps.odoo.com/apps/modules/18.0/report_templateto)** on the Odoo Apps Store (Odoo 18, supported on Odoo.sh and self-hosted; not available on Odoo Online).

1. Visit the [TemplateTo Reports listing](https://apps.odoo.com/apps/modules/18.0/report_templateto)
2. Click **Buy / Download** and follow Odoo's prompts to install on your instance
3. Or, from your Odoo dashboard: go to **Apps**, search for **"TemplateTo"**, and click **Install**

### Manual Installation

1. Clone or copy the `report_templateto` directory into your Odoo addons path:
   ```bash
   cp -r report_templateto /opt/odoo/addons/
   ```
2. Restart Odoo
3. Go to **Settings → Technical → Update Apps List**
4. Search for "TemplateTo" in the Apps list and click **Install**

## Configuration

Once installed, configure the module in Odoo Settings:

1. Go to **Settings → TemplateTo** (under the Technical section)
2. Enable **"Use TemplateTo for PDF Rendering"**
3. Enter your **API key**
4. Click **Test Connection** to verify the key works
5. Click **Save**

![TemplateTo settings in Odoo](../../images/odoo-settings.png)

!!! tip
    Don't have an API key yet? [Create a free account](https://app.templateto.com) and generate one from **Settings → API Keys**. See the [API keys documentation](../../account/api-keys.md) for details.

### Settings Reference

| Setting | Default | Description |
|---------|---------|-------------|
| Enable TemplateTo | Off | Global toggle — when off, Odoo uses wkhtmltopdf as normal |
| API Key | — | Your TemplateTo API key |
| API Endpoint | `https://api.templateto.com` | Only change if directed by TemplateTo support |
| Fallback to wkhtmltopdf | On | Automatically use wkhtmltopdf if the TemplateTo API is unreachable |
| Batch threshold | 20 | Number of records above which batch generation is used instead of sync |

## Printing a Single Report

Once the module is enabled, printing works exactly as before — but PDFs are now rendered by Chrome headless instead of wkhtmltopdf.

1. Open any record (e.g. an invoice, sale order, purchase order)
2. Click **Print** and select the report (e.g. "Invoices")
3. The PDF downloads as usual

![Invoice with Print button in Odoo](../../images/odoo-invoice.png)

Behind the scenes:

1. Odoo renders the QWeb template to HTML (unchanged)
2. The module sends the HTML to the TemplateTo API
3. TemplateTo renders it with Chromium and returns PDF bytes
4. The PDF is returned to Odoo as if wkhtmltopdf had generated it

For small batches (up to 20 records by default), the module sends parallel API requests for fast results.

## Batch PDF Generation

When you need to generate PDFs for many records at once — month-end invoices, bulk statements, mass mailings — the module uses background processing to avoid browser timeouts.

### How to Run a Batch Job

1. Go to a list view (e.g. **Invoicing → Invoices**)
2. Select the records you want (use the checkboxes)
3. Click **Actions → Generate PDFs via TemplateTo**
4. The batch wizard opens — confirm and start the job

### What Happens Next

- A background job is created and processed by Odoo's scheduled actions
- Each record's report HTML is sent to the TemplateTo async API
- Progress is tracked and visible in the batch job view
- When complete, you receive a notification with a link to download all PDFs
- Each generated PDF is attached to its source record — you'll find it in the **Files** section of the record's chatter

![Invoice with generated PDF attached](../../images/odoo-invoice-with-pdf.png)

### Viewing Batch Jobs

!!! note
    The batch jobs menu is under **Settings → Technical**, which is only visible when [developer mode](https://www.odoo.com/documentation/18.0/applications/general/developer_mode.html) is enabled. Activate it via **Settings → Developer Tools → Activate the developer mode**.

Go to **Settings → Technical → TemplateTo Batch Jobs** to see all batch jobs, their progress, and download completed results.

![Batch PDF Jobs list in Odoo](../../images/odoo-batch-jobs.png)

## Fallback Behaviour

When **"Fallback to wkhtmltopdf"** is enabled (the default):

- If the TemplateTo API returns an error or is unreachable, the module silently falls back to wkhtmltopdf
- The PDF is generated as if the module weren't installed
- A warning is logged in the Odoo server log for debugging

When fallback is **disabled**:

- API failures raise an error to the user
- This is useful if you want to ensure all PDFs are rendered by Chromium and be alerted to any issues

## Data & Privacy

The module sends **only rendered report HTML** to the TemplateTo API. This is the same HTML that wkhtmltopdf would receive — no Odoo credentials, database names, user data, or metadata are transmitted.

How your data is handled depends on the rendering mode:

- **Sync rendering** (Print button, small batches) — your HTML is processed in [volatile memory](https://templateto.com/security) only, meaning it exists temporarily in the server's working memory and is never saved to disk. The data is discarded as soon as the PDF is returned.
- **Async rendering** (background batch jobs) — the resulting PDF is stored temporarily in encrypted S3 storage for up to **24 hours** so your Odoo server can retrieve it. After 24 hours, the file is automatically deleted. The source HTML is still processed in volatile memory only and is never persisted to disk.

For full details, see the [TemplateTo Security page](https://templateto.com/security).

The module is disabled by default and requires explicit opt-in before any data is sent.

## Troubleshooting

### "Connection Test Failed"

- Verify your API key is correct — copy it fresh from [app.templateto.com](https://app.templateto.com)
- Check that your Odoo server can reach `https://api.templateto.com` (firewall/proxy rules)
- If using a custom endpoint, verify the URL is correct

### PDFs Look the Same as Before

- Make sure the module is **enabled** in Settings → TemplateTo
- Check the Odoo server log for TemplateTo-related messages
- If fallback is enabled, the module may be silently falling back to wkhtmltopdf due to an API issue

### Batch Job Stuck at 0%

- Verify that Odoo's scheduled actions (cron jobs) are running
- Check the Odoo server log for errors in the `report_templateto` module
- Ensure your API key has sufficient quota for the number of records

### Timeout on Large Reports

- For very large reports, the sync API has a timeout. The module automatically uses batch mode for record counts above the configured threshold (default: 20)
- You can lower the batch threshold in settings if needed

### General Debugging

Enable Odoo's debug mode and check the server log for lines containing `templateto`. The module logs API requests, responses, and any fallback events.
