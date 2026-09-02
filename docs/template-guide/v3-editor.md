# The Modern Editor (V3)

New templates open in the modern editor. It keeps everything the classic editor can do, but organises the workspace around three modes so you always know which view you are editing in — and it adds the [AI Assistant](ai-assistant.md).

!!! note
    Existing templates keep the editor they were created with. The editor version is shown in Template Settings; templates created before V3 continue to use the classic editor described in the [Editor Overview](editor-overview.md).

## The three modes

The centre of the top bar switches between three modes:

| Mode | What it is for |
| ---- | -------------- |
| **Design** | The drag-and-drop canvas. Drop elements from the palette, select anything to style it, and see the document roughly as it will print. |
| **Code** | The same template as source, in three tabs — **Data** (sample JSON), **HTML** (the template with Liquid) and **CSS** — with a live preview beside them. |
| **Preview** | The rendered output. The **HTML** tab is instant and composed exactly as the renderer sees it; the **PDF** tab produces the true PDF via the real rendering pipeline. |

Switching between Design and Code is lossless in both directions: components you placed on the canvas — tables, repeating sections, conditionals, QR codes — keep their settings when you edit the HTML and switch back. Edit whichever view suits the change you are making.

!!! tip
    Small content tweaks are often quickest in Code mode; layout work is quickest in Design mode. The editor does not care which you use — it is the same template underneath.

## The top bar

From left to right:

- **Design | Code | Preview** — the mode switch. Switching away from Code first applies your code changes (invalid JSON in the Data tab will stop the switch so nothing is lost).
- **Ask AI** — opens the [AI Assistant](ai-assistant.md) dock.
- **Save** — saves a draft version, exactly as in the classic editor.
- **Published / Draft** — the version status pill, with publishing controls.
- **⋯** — Settings, History and Help.

## The AI Assistant dock

The panel on the right edge of the editor is the AI Assistant. When collapsed it is a slim rail labelled *Assistant*; click it (or the **Ask AI** button) to open it. It can propose edits to the template's HTML, CSS **and** sample data, and nothing it proposes touches your template until you apply it — see [AI Assistant](ai-assistant.md) for the full guide.

## What is different from the classic editor?

- One place for everything: HTML and CSS are no longer split across separate side panels — Code mode holds Data, HTML and CSS together beside a live preview.
- Preview is a mode, not a separate tab in your browser.
- The AI Assistant is built in.
- Everything you place in Design mode survives a round-trip through Code mode and back.
