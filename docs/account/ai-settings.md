# AI Settings

The **AI Assistant** section of the [Account page](https://app.templateto.com/account) controls which AI provider powers [Ask AI](../template-guide/ai-assistant.md) in the template editor, and the privacy rules around it.

!!! note
    AI editing is included on paid plans. On TemplateTo Lite the section explains the upgrade instead.

## Provider and key

Choose who serves your company's AI requests:

| Provider | Notes |
| -------- | ----- |
| **Platform default** | Uses TemplateTo's configuration. When no platform key is set up, AI editing stays off until you add your own key. |
| **OpenAI** | Bring your own OpenAI API key. |
| **OpenRouter** | One key that routes to many upstream models — with the privacy routing described below. |
| **Anthropic** | Bring your own Anthropic API key. |

Your API key is **write-only**: it is stored encrypted and never shown again. The *key saved* badge confirms one is on file; enter a new key to replace it, or tick *Remove the saved key* to fall back to the platform key.

The **Model** field is optional — leave it blank for the provider's default, or pin a specific model (for OpenRouter, any OpenRouter model id such as `openai/gpt-4o`).

## OpenRouter privacy routing

When OpenRouter is the effective provider, every request is sent with **zero-data-retention endpoints only** and **data collection denied**. This is enforced by the platform and is not configurable — some models may be unavailable under these rules.

Two routing options are yours to tune:

- **Allowed providers** — a comma-separated allowlist of OpenRouter upstream providers; only these serve your requests. Blank allows any zero-data-retention provider.
- **Allow fallbacks** — off by default, meaning a request fails rather than routing outside your allowlist. Turn it on to let OpenRouter fall back to another (still zero-data-retention) provider when yours are unavailable.

## What is sent, and what is kept

Every Ask AI request sends your template's HTML, stylesheet and sample data (values included) to the provider you chose — that is what lets the assistant edit all three.

What TemplateTo *keeps* is separate, and controlled by the **full-context logging** toggle:

- **Off (default)** — the AI audit log records only your instruction, token counts and outcome.
- **On** — complete prompts and responses (including sample-data values) are retained in the audit log.

## Troubleshooting

- *"AI editing is not enabled"* — no provider is configured: pick one and add a key.
- *"The stored AI API key can no longer be decrypted"* — re-enter your key; this appears only after a platform-side encryption key rotation.
- *"The AI provider did not respond within 240 seconds"* — the model was too slow. Try again, choose a faster model, or loosen restrictive OpenRouter routing.
- *"The daily limit of AI edits has been reached"* — applies only on the platform key; add your own key for unlimited use.
