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

See [API Key Management](../getting-started/api-key-management.md) for details on creating API keys.

## Endpoints

### Start Async Job (Template)

Start a background document generation job using a template.

```
POST /render/async/{renderType}/{templateId}
```

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `renderType` | string | Document type: `pdf`, `docx`, or `txt` |
| `templateId` | string | Your template ID |

#### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `templateVersionId` | string | Optional. Specific template version to use |

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

Start a background PDF generation job from raw HTML content.

```
POST /render/async/{renderType}/fromhtml
```

#### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `renderType` | string | Currently only `pdf` is supported |

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
  "resultUrl": "/render/async/result/01HQXYZ123456789ABCDEF"
}
```

!!! note "ResultUrl"
    The `resultUrl` field is only included when `status` is `Completed`.

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

## Job Statuses

| Status | Description |
|--------|-------------|
| `Pending` | Job is queued and waiting to be processed |
| `Processing` | Document generation is in progress |
| `Completed` | Document is ready for download |
| `Failed` | An error occurred during generation |
| `Expired` | Result is no longer available (24 hours passed) |

## Complete Example

Here's a complete workflow in JavaScript:

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

## Error Handling

### Common Error Responses

| Status Code | Meaning |
|-------------|---------|
| 400 | Invalid request (bad render type, missing data, job not completed) |
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
