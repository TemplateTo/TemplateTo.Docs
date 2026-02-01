# Async Rendering API

The async rendering API allows you to generate documents in the background, which is ideal for large documents, batch processing, or when you don't want to block your application waiting for a response.

## Overview

Unlike synchronous rendering which returns the document directly, async rendering:

1. **Starts a background job** - Returns immediately with a job ID
2. **Processes in the background** - Document generation happens asynchronously
3. **Provides status polling** - Check if your document is ready
4. **Returns a download URL** - Fetch the completed document

!!! info "Result Expiration"
    Completed documents are available for download for **24 hours** after generation. After this period, you'll need to render the document again.

## Authentication

All async endpoints require an API key. Pass your API key in the `X-Api-Key` header.

See [Authentication](authentication.md) for details on creating and managing API keys.

## Endpoints

### Start Async Job (Template)

Start a background document generation job using a template.

```
POST /render/async/{renderType}/{templateId}
```

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `renderType` | string | Document type: `pdf`, `docx`, `txt`, or `image` |
| `templateId` | string | Your template ID |

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `templateVersionId` | string | Optional. Specific template version to use |
| `outputPresignedUrl` | string | Optional. Presigned PUT URL where TemplateTo will upload the result instead of our S3 storage |
| `webhookUrl` | string | Optional. HTTPS URL to receive a POST notification when the job completes |

#### Image-Specific Options

When using `renderType=image`, you can pass additional options in the request body:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `format` | string | `"png"` | Output format: `"png"` or `"jpeg"` |
| `width` | number | Template default | Image width in pixels |
| `height` | number | Auto | Image height in pixels (only when `fullPage=false`) |
| `quality` | number | `90` | JPEG quality (0-100, only for JPEG format) |
| `fullPage` | boolean | `true` | Capture full scrollable page |
| `scale` | number | `1` | Device scale factor (use `2` for retina) |

**Example image request body:**
```json
{
  "customerName": "Acme Corp",
  "format": "jpeg",
  "width": 1200,
  "quality": 85,
  "scale": 2
}
```

#### Request Body

Pass your template data as JSON:

```json
{
  "customerName": "Acme Corp",
  "invoiceNumber": "INV-2024-001",
  "items": [
    { "description": "Widget", "quantity": 5, "price": 10.00 },
    { "description": "Gadget", "quantity": 2, "price": 25.00 }
  ]
}
```

#### Response (202 Accepted)

```json
{
  "jobId": "01HQXYZ123456789ABCDEF",
  "status": "Pending",
  "templateId": "tpl_abc123",
  "renderType": "pdf",
  "createdAt": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-16T10:30:00Z",
  "statusUrl": "/render/async/status/01HQXYZ123456789ABCDEF",
  "resultUrl": "/render/async/result/01HQXYZ123456789ABCDEF"
}
```

---

### Start Async Job (Raw HTML)

Start a background document generation job from raw HTML content.

```
POST /render/async/{renderType}/fromhtml
```

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `renderType` | string | `pdf` or `image` |

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `outputPresignedUrl` | string | Optional. Presigned PUT URL where TemplateTo will upload the result instead of our S3 storage |
| `webhookUrl` | string | Optional. HTTPS URL to receive a POST notification when the job completes |

#### Request Body

```json
{
  "base64HtmlString": "PGh0bWw+PGJvZHk+SGVsbG8gV29ybGQ8L2JvZHk+PC9odG1sPg=="
}
```

!!! tip "Base64 Encoding"
    The HTML content must be Base64 encoded. In JavaScript: `btoa(htmlString)`. In Python: `base64.b64encode(html_string.encode()).decode()`.

#### Response (202 Accepted)

Same format as template-based jobs, but `templateId` will be `null`.

---

### Start Image Clips Job (Template)

Start a background job to extract multiple image clips from a single template render.

```
POST /render/async/image/{templateId}/clips
```

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `templateId` | string | Your template ID |

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `templateVersionId` | string | Optional. Specific template version to use |
| `outputPresignedUrl` | string | Optional. Presigned PUT URL where TemplateTo will upload the result instead of our S3 storage |
| `webhookUrl` | string | Optional. HTTPS URL to receive a POST notification when the job completes |

#### Request Body

```json
{
  "data": {
    "customerName": "Acme Corp",
    "logoUrl": "https://example.com/logo.png"
  },
  "clips": [
    { "name": "header", "x": 0, "y": 0, "width": 800, "height": 200 },
    { "name": "logo", "x": 10, "y": 10, "width": 100, "height": 100, "circular": true }
  ],
  "format": "png",
  "width": 1200,
  "quality": 90
}
```

#### Clip Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | string | Identifier for this clip in the result |
| `x` | number | X coordinate (pixels from left) |
| `y` | number | Y coordinate (pixels from top) |
| `width` | number | Clip width in pixels |
| `height` | number | Clip height in pixels |
| `circular` | boolean | Optional. Apply circular/elliptical mask (forces PNG output) |
| `format` | string | Optional. Override format for this clip (`"png"` or `"jpeg"`) |

!!! info "Circular Clips"
    When `circular: true` is set, the clip will have a circular (or elliptical if width != height) mask with transparent background. This automatically forces PNG output for that clip, regardless of the requested format.

#### Response (202 Accepted)

```json
{
  "jobId": "01HQXYZ123456789ABCDEF",
  "status": "Pending",
  "templateId": "tpl_abc123",
  "renderType": "image-clips",
  "createdAt": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-16T10:30:00Z",
  "statusUrl": "/render/async/status/01HQXYZ123456789ABCDEF",
  "resultUrl": "/render/async/result/01HQXYZ123456789ABCDEF"
}
```

---

### Start Image Clips Job (Raw HTML)

Start a background job to extract multiple image clips from raw HTML content.

```
POST /render/async/image/fromhtml/clips
```

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `outputPresignedUrl` | string | Optional. Presigned PUT URL where TemplateTo will upload the result instead of our S3 storage |
| `webhookUrl` | string | Optional. HTTPS URL to receive a POST notification when the job completes |

#### Request Body

```json
{
  "base64HtmlString": "PGh0bWw+PGJvZHk+PGRpdiBpZD0iaGVhZGVyIj5IZWxsbzwvZGl2PjwvYm9keT48L2h0bWw+",
  "clips": [
    { "name": "header", "x": 0, "y": 0, "width": 800, "height": 200 },
    { "name": "logo", "x": 10, "y": 10, "width": 100, "height": 100, "circular": true }
  ],
  "format": "png",
  "width": 1200,
  "quality": 90
}
```

#### Response (202 Accepted)

Same format as template-based clips jobs, but `templateId` will be `null`.

---

### Check Job Status

Poll this endpoint to check if your document is ready.

```
GET /render/async/status/{jobId}
```

#### Response (200 OK)

```json
{
  "jobId": "01HQXYZ123456789ABCDEF",
  "status": "Completed",
  "templateId": "tpl_abc123",
  "renderType": "pdf",
  "createdAt": "2024-01-15T10:30:00Z",
  "completedAt": "2024-01-15T10:30:05Z",
  "expiresAt": "2024-01-16T10:30:00Z",
  "elapsedTimeMs": 5234,
  "resultUrl": "/render/async/result/01HQXYZ123456789ABCDEF",
  "webhookDelivered": true,
  "usesExternalOutput": false
}
```

| Field | Description |
|-------|-------------|
| `resultUrl` | Only included when `status` is `Completed` |
| `webhookDelivered` | `true` if webhook was sent successfully, `false` if delivery failed, `null` if no webhook configured |
| `usesExternalOutput` | `true` if a presigned URL was provided and the result was uploaded there |

!!! note "External Output"
    When `usesExternalOutput` is `true`, the `resultUrl` will still be provided but may redirect to an empty response since the actual result was uploaded to your presigned URL.

---

### Get Result

Retrieve the completed document. This endpoint redirects to a temporary download URL.

```
GET /render/async/result/{jobId}
```

#### Response

- **302 Found** - Redirects to a pre-signed download URL
- **400 Bad Request** - Job is not yet completed
- **404 Not Found** - Job not found or expired

---

## Delivery Options

By default, async rendering stores results in TemplateTo's S3 storage and provides a download URL. You can customize how results are delivered using the following options.

### Default Delivery (S3 Download)

Without any additional parameters, completed documents are stored in our S3 bucket. Use the `resultUrl` from the status response to download via a pre-signed URL.

### Custom Storage (Presigned URL)

Upload results directly to your own cloud storage by providing an `outputPresignedUrl` parameter.

!!! tip "When to Use"
    Use presigned URLs when you want results in your own storage, need to avoid an extra download step, or have compliance requirements about where data is stored.

#### Requirements

- **HTTPS only** - The presigned URL must use HTTPS
- **PUT method** - The URL must allow HTTP PUT uploads

#### Supported Providers

| Provider | Example URL Pattern |
|----------|---------------------|
| AWS S3 | `https://bucket.s3.region.amazonaws.com/key?X-Amz-...` |
| Azure Blob Storage | `https://account.blob.core.windows.net/container/blob?sv=...` |
| Google Cloud Storage | `https://storage.googleapis.com/bucket/object?X-Goog-...` |
| DigitalOcean Spaces | `https://bucket.region.digitaloceanspaces.com/key?X-Amz-...` |
| Wasabi | `https://bucket.s3.region.wasabisys.com/key?X-Amz-...` |
| Backblaze B2 | `https://bucket.s3.region.backblazeb2.com/key?X-Amz-...` |
| Cloudflare R2 | `https://account.r2.cloudflarestorage.com/bucket/key?X-Amz-...` |

#### Example: Generate Presigned URL (AWS SDK)

```javascript
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';

const s3 = new S3Client({ region: 'us-east-1' });

const presignedUrl = await getSignedUrl(s3, new PutObjectCommand({
  Bucket: 'my-bucket',
  Key: `renders/${Date.now()}.pdf`,
  ContentType: 'application/pdf'
}), { expiresIn: 3600 });
```

### Webhook Notifications

Receive a notification when your job completes instead of polling.

!!! tip "When to Use"
    Use webhooks for event-driven architectures, serverless workflows, or when you want to avoid polling overhead.

#### Requirements

- **HTTPS only** - The webhook URL must use HTTPS
- **POST method** - Your endpoint must accept POST requests
- **Respond quickly** - Return a 2xx status code within 30 seconds

See [Webhook Reference](#webhook-reference) for payload format and delivery details.

### Combining Options

You can use both `outputPresignedUrl` and `webhookUrl` together for a fully push-based workflow where:

1. Job completes and result uploads to your storage
2. Webhook notifies your application
3. Your app processes the file directly from your storage

```
POST /render/async/pdf/{templateId}?outputPresignedUrl={url}&webhookUrl={webhook}
```

---

## Webhook Reference

When you provide a `webhookUrl`, TemplateTo sends a POST request to that URL when the job completes (whether successful or failed).

### Payload Format

```json
{
  "jobId": "01HQXYZ123456789ABCDEF",
  "status": "Completed",
  "renderType": "pdf",
  "templateId": "tpl_abc123",
  "completedAt": "2024-01-15T10:30:05Z",
  "elapsedTimeMs": 5234,
  "errorMessage": null,
  "resultUrl": "https://api.templateto.com/render/async/result/01HQXYZ123456789ABCDEF"
}
```

| Field | Type | Description |
|-------|------|-------------|
| `jobId` | string | The job identifier |
| `status` | string | `Completed` or `Failed` |
| `renderType` | string | `pdf`, `image`, `image-clips`, `docx`, or `txt` |
| `templateId` | string | Template ID (null for raw HTML jobs) |
| `completedAt` | string | ISO 8601 timestamp of completion |
| `elapsedTimeMs` | number | Total processing time in milliseconds |
| `errorMessage` | string | Error details if `status` is `Failed`, otherwise null |
| `resultUrl` | string | URL to download the result (only for successful jobs) |

### Delivery Behavior

- Webhooks are sent on both **success** and **failure**
- Automatic retries on delivery failure (up to 3 attempts with exponential backoff)
- Delivery is considered successful on any 2xx HTTP response
- Check `webhookDelivered` in the status response to verify delivery

!!! warning "Idempotency"
    Your webhook endpoint may receive the same notification multiple times due to retries. Design your handler to be idempotent.

---

## Job Statuses

| Status | Description |
|--------|-------------|
| `Pending` | Job is queued and waiting to be processed |
| `Processing` | Document generation is in progress |
| `Completed` | Document is ready for download |
| `Failed` | An error occurred during generation |
| `Expired` | Result is no longer available (24 hours passed) |

---

## Complete Examples

### Polling Workflow

Here's a complete workflow using polling to wait for completion:

```javascript
const API_BASE = 'https://api.templateto.com';
const API_KEY = 'your-api-key';

async function generateDocumentAsync(templateId, data) {
  // 1. Start the job
  const startResponse = await fetch(
    `${API_BASE}/render/async/pdf/${templateId}`,
    {
      method: 'POST',
      headers: {
        'X-Api-Key': API_KEY,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
    }
  );

  const job = await startResponse.json();
  console.log(`Job started: ${job.jobId}`);

  // 2. Poll for completion
  let status = job.status;
  while (status === 'Pending' || status === 'Processing') {
    await new Promise(resolve => setTimeout(resolve, 1000)); // Wait 1 second

    const statusResponse = await fetch(
      `${API_BASE}/render/async/status/${job.jobId}`,
      { headers: { 'X-Api-Key': API_KEY } }
    );

    const statusData = await statusResponse.json();
    status = statusData.status;
    console.log(`Status: ${status}`);
  }

  if (status !== 'Completed') {
    throw new Error(`Job failed with status: ${status}`);
  }

  // 3. Get the result (follow redirect to download)
  const resultResponse = await fetch(
    `${API_BASE}/render/async/result/${job.jobId}`,
    {
      headers: { 'X-Api-Key': API_KEY },
      redirect: 'follow'
    }
  );

  return await resultResponse.blob();
}

// Usage
const pdfBlob = await generateDocumentAsync('tpl_abc123', {
  customerName: 'Acme Corp',
  total: 150.00
});
```

### Webhook Workflow

Use webhooks to avoid polling entirely:

```javascript
const API_BASE = 'https://api.templateto.com';
const API_KEY = 'your-api-key';
const WEBHOOK_URL = 'https://your-app.com/webhooks/templateto';

async function startAsyncJob(templateId, data) {
  const response = await fetch(
    `${API_BASE}/render/async/pdf/${templateId}?webhookUrl=${encodeURIComponent(WEBHOOK_URL)}`,
    {
      method: 'POST',
      headers: {
        'X-Api-Key': API_KEY,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
    }
  );

  const job = await response.json();
  console.log(`Job started: ${job.jobId}`);
  // Store job.jobId in your database to correlate with webhook
  return job;
}

// Your webhook handler (Express.js example)
app.post('/webhooks/templateto', async (req, res) => {
  const { jobId, status, resultUrl, errorMessage } = req.body;

  if (status === 'Completed') {
    // Download and process the result
    const pdfResponse = await fetch(resultUrl, {
      headers: { 'X-Api-Key': API_KEY },
      redirect: 'follow'
    });
    const pdfBuffer = await pdfResponse.arrayBuffer();
    // Process the PDF...
  } else {
    console.error(`Job ${jobId} failed: ${errorMessage}`);
  }

  res.status(200).send('OK');
});
```

### Direct Upload to Your S3

Upload results directly to your own S3 bucket:

```javascript
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';

const API_BASE = 'https://api.templateto.com';
const API_KEY = 'your-api-key';
const s3 = new S3Client({ region: 'us-east-1' });

async function renderToMyS3(templateId, data, s3Key) {
  // 1. Generate a presigned URL for your S3 bucket
  const presignedUrl = await getSignedUrl(s3, new PutObjectCommand({
    Bucket: 'my-documents-bucket',
    Key: s3Key,
    ContentType: 'application/pdf'
  }), { expiresIn: 3600 });

  // 2. Start the job with the presigned URL
  const response = await fetch(
    `${API_BASE}/render/async/pdf/${templateId}?outputPresignedUrl=${encodeURIComponent(presignedUrl)}`,
    {
      method: 'POST',
      headers: {
        'X-Api-Key': API_KEY,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
    }
  );

  const job = await response.json();

  // 3. Poll for completion (or use webhook)
  let status = job.status;
  while (status === 'Pending' || status === 'Processing') {
    await new Promise(resolve => setTimeout(resolve, 1000));
    const statusResponse = await fetch(
      `${API_BASE}/render/async/status/${job.jobId}`,
      { headers: { 'X-Api-Key': API_KEY } }
    );
    const statusData = await statusResponse.json();
    status = statusData.status;
  }

  if (status === 'Completed') {
    console.log(`PDF uploaded to s3://my-documents-bucket/${s3Key}`);
  }
}

// Usage
await renderToMyS3('tpl_invoice', invoiceData, `invoices/${invoiceId}.pdf`);
```

---

## Error Handling

### Common Error Responses

| Status Code | Meaning |
|-------------|---------|
| 400 | Invalid request (bad render type, missing data, job not completed, invalid presigned URL) |
| 401 | Authentication required or invalid |
| 404 | Template or job not found |
| 500 | Internal server error |

### Error Response Format

```json
{
  "error": "Description of what went wrong"
}
```

!!! warning "Failed Jobs"
    If a job fails, check the `errorMessage` field in the status response for details about what went wrong.

### Presigned URL Errors

If the presigned URL upload fails, the job will complete with a `Failed` status and the error message will indicate the upload issue (e.g., expired URL, access denied, invalid URL format).

### Webhook Delivery Failures

If webhook delivery fails after all retry attempts:

- The job still completes normally
- `webhookDelivered` will be `false` in the status response
- You can still retrieve the result via the normal download flow
