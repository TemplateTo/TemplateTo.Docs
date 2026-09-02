# TemplateTo Lite

TemplateTo Lite is the free plan behind our [JSON-to-PDF tool](https://app.templateto.com/tools/json-to-pdf). No card required — sign up from the tool, download your PDF, and the same account works with the API straight away.

## What is included

| | TemplateTo Lite |
| --- | --- |
| PDF renders | 25 each month |
| Templates | 1 |
| API keys | 1 (created for you at signup) |
| Content blocks & custom fonts | Not included |
| AI Assistant | Not included — available on paid plans |

Usage is shown on your [Account page](https://app.templateto.com/account), along with the upgrade options when you need more.

## The JSON-to-PDF tool

The [tool](https://app.templateto.com/tools/json-to-pdf) runs entirely in your browser until you download: paste JSON on the **Data** tab, adjust the HTML + Liquid template on the **Template** tab, and the preview updates as you type. Downloading the real PDF uses one of your monthly renders.

Starters are available for common documents — append `?template=invoice`, `?template=quote` or `?template=receipt` to the tool's URL.

## The same render, from code

Your account includes an API key (see [API Keys](../account/api-keys.md)). The tool's exact render is available as an endpoint:

```bash
curl --request POST https://api.templateto.com/free-tools/json-to-pdf \
  --header "X-API-KEY: YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{"htmlTemplate":"<h1>{{ title }}</h1>","data":{"title":"Hello from TemplateTo"}}'
```

The response is the PDF itself. Each call uses one monthly render.

!!! note
    Lite renders are sized for trying things out: template and data are limited to 100,000 characters each, and renders run one at a time. The full [REST API](../developer-guide/rest-api.md) — saved templates, versioning, DOCX/TXT output, async rendering — is available on paid plans.

## When you outgrow it

Everything you build on Lite carries over: your template, your API key and your account stay exactly as they are when you upgrade. Head to your [Account page](https://app.templateto.com/account) and choose **Upgrade Plan**.
