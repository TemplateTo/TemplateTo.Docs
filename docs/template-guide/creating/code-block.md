# Code Block

The **Code Block** element lets you insert raw code, Liquid templates, or HTML directly into your template. Unlike the [HTML element](elements.md#html), the Code Block's content passes through to the final document without any wrapping `<div>` — making it ideal for Liquid logic and dynamic content.

## Adding a Code Block

1. Open the **Elements** panel on the left
2. Find **Code Block** in the **Advanced** category
3. Drag it onto your template canvas

The Code Block appears as a dashed-border box in the editor. This border is only visible while editing — it won't appear in your generated documents.

## Editing Content

There are two ways to open the code editor:

- **Double-click** the Code Block on the canvas
- **Click** the Code Block, then click the **cog icon** (⚙) in the toolbar

This opens a full-screen code editor with syntax highlighting. Write your code, then click **Save** to apply changes.

## When to Use Code Block vs HTML

| Feature | Code Block | HTML |
|---|---|---|
| Liquid/dynamic content | ✅ Yes — content passes through unmodified | ❌ No — HTML is stored as static editor components |
| Wrapping element | None — outputs raw content | Wrapped in a `<div>` |
| Visual editing | Code editor only | Visual drag-and-drop |
| Best for | Liquid logic, dynamic HTML, loops, conditionals | Static HTML that needs visual layout control |

!!! tip
    **Use Code Block for Liquid content.** If you need `{% for %}` loops, `{% if %}` conditionals, or variable output like `{{ customer.name }}`, the Code Block is the right choice. The HTML element stores content as editor components which can interfere with Liquid syntax.

## Example: Liquid Loop

Drag a Code Block onto your template and enter:

```liquid
{% for item in order.items %}
<tr>
    <td>{{ item.name }}</td>
    <td>{{ item.quantity }}</td>
    <td>{{ item.price | currency }}</td>
</tr>
{% endfor %}
```

When the template is rendered with data, this produces a table row for each item in the order.

## Example: Conditional Content

```liquid
{% if customer.vip %}
    <div style="background: #f0f9ff; padding: 16px; border-radius: 8px;">
        <strong>Thank you for being a VIP customer!</strong>
    </div>
{% endif %}
```

## Scripting Limitations

!!! warning
    `<script>` tags are automatically removed from Code Block content for security. Code Blocks are designed for HTML markup and Liquid template logic, not JavaScript execution.

## Related

- [Elements Reference](elements.md) — overview of all available elements
- [Liquid Reference](../dynamic-content/liquid-reference.md) — full Liquid syntax guide
- [Variables & Data](../dynamic-content/variables.md) — passing data to templates
