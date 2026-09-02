# AI Assistant

The AI Assistant ("Ask AI") lives in the right-hand dock of the [modern editor](v3-editor.md). Describe a change in plain language and it proposes an edit — to the template's HTML, its CSS, and its sample data — which you review before anything is applied.

!!! note
    AI editing is included on paid plans. You will also need an AI provider configured in [AI Settings](../account/ai-settings.md) — either your own API key or the platform default when one is available.

## Asking for a change

1. Open the dock with the **Ask AI** button (or click the *Assistant* rail on the right edge).
2. Optionally set the **scope**: select an element on the canvas (or click one in Code mode's live preview) and the request targets that element. With nothing selected, the whole template is in scope.
3. Describe the change — for example *"show a red PAST DUE stamp when the invoice is overdue"* or *"add a customer greeting using a new customerName field in the sample data"*.
4. Click **Propose edit**. Slower models can take a minute or two; you can **Cancel** at any point.

## Reviewing a proposal

A proposal shows the current and proposed HTML side by side, plus **Proposed CSS** and **Proposed data** sections whenever those changed. Two buttons:

- **Apply as draft** — applies the proposal to the editor. It is now a normal unsaved change: preview it, adjust it, undo it, or discard by leaving without saving. Nothing is saved to the template until you click **Save**.
- **Discard** — throws the proposal away.

!!! tip
    The assistant understands the editor's building blocks. Ask for "a conditional section", "a repeating list" or "a table" and the result is a real editor component — selectable and editable through its own settings dialog afterwards, not just raw markup.

## What the assistant can see

Each request sends the template's HTML, its stylesheet, and the sample data (values included) to the AI provider configured for your company, together with your instruction and the selected element if you set a scope. It does not see other templates, your account details, or your generated documents.

Where those requests go — and the privacy controls around them — is configured in [AI Settings](../account/ai-settings.md).

## Limits

- Proposals are validated before you see them: the returned template must be valid Liquid and the data must be valid JSON, otherwise the assistant is asked once to correct itself.
- Requests time out after four minutes.
- When the platform key is in use (no company key configured), a daily per-company limit applies.
