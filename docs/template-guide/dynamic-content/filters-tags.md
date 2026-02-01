# Filters & Tags

TemplateTo extends Liquid with custom filters and tags for document generation. These work alongside [standard Liquid filters](https://shopify.github.io/liquid/filters/abs/).

## Filters

Filters transform values. Apply them with the pipe (`|`) character:

```liquid
{{ value | filter_name }}
{{ value | filter_name: argument }}
```

### parse_date

Converts a string to a date object, which you can then format with the `date` filter.

**Syntax:**
```liquid
{{ string | parse_date }}
{{ string | parse_date: true }}  <!-- US date format (MM/DD/YY) -->
```

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `isUS` | boolean | `false` | Set to `true` for US date format (MM/DD/YY) |

**Examples:**

```liquid
<!-- European format (DD/MM/YY) - default -->
{{ "15/11/23" | parse_date | date: "%e %B %Y" }}
<!-- Result: 15 November 2023 -->

<!-- US format (MM/DD/YY) -->
{{ "11/15/23" | parse_date: true | date: "%e %B %Y" }}
<!-- Result: 15 November 2023 -->

<!-- Different output formats -->
{{ "15/11/23" | parse_date | date: "%Y-%m-%d" }}
<!-- Result: 2023-11-15 -->

{{ "15/11/23" | parse_date | date: "%d/%m/%Y" }}
<!-- Result: 15/11/2023 -->
```

!!! tip "Date Format Reference"
    Use [strftime.net](https://strftime.net/) to build date format strings interactively.

**Common date format codes:**

| Code | Description | Example |
|------|-------------|---------|
| `%Y` | 4-digit year | 2023 |
| `%y` | 2-digit year | 23 |
| `%m` | Month (01-12) | 11 |
| `%B` | Full month name | November |
| `%b` | Short month name | Nov |
| `%d` | Day (01-31) | 15 |
| `%e` | Day (1-31, space-padded) | 15 |
| `%H` | Hour (00-23) | 14 |
| `%I` | Hour (01-12) | 02 |
| `%M` | Minute (00-59) | 30 |
| `%p` | AM/PM | PM |

---

### format_number

Formats numbers using [.NET standard numeric format strings](https://learn.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings).

**Syntax:**
```liquid
{{ number | format_number: "format_string" }}
```

**Examples:**

```liquid
<!-- Currency -->
{{ 1234.5 | format_number: "C" }}
<!-- Result: $1,234.50 (depends on culture) -->

<!-- Fixed decimal places -->
{{ 123 | format_number: "N" }}
<!-- Result: 123.00 -->

{{ 123.456 | format_number: "N2" }}
<!-- Result: 123.46 -->

<!-- Percentage -->
{{ 0.25 | format_number: "P" }}
<!-- Result: 25.00% -->

<!-- Custom format -->
{{ 1234567.89 | format_number: "#,##0.00" }}
<!-- Result: 1,234,567.89 -->
```

**Common format specifiers:**

| Format | Description | Example Input | Example Output |
|--------|-------------|---------------|----------------|
| `N` | Number with thousand separators | 1234.5 | 1,234.50 |
| `N0` | Number, no decimals | 1234.5 | 1,235 |
| `N2` | Number, 2 decimals | 1234.5 | 1,234.50 |
| `C` | Currency | 1234.5 | $1,234.50 |
| `C0` | Currency, no decimals | 1234.5 | $1,235 |
| `P` | Percentage | 0.25 | 25.00% |
| `P0` | Percentage, no decimals | 0.256 | 26% |
| `F2` | Fixed-point, 2 decimals | 1234.5 | 1234.50 |

!!! note "Culture-Aware Formatting"
    Currency symbols and separators depend on the template's [culture settings](localization.md). For example, `C` displays `$` for `en-US` but `£` for `en-GB`.

---

### to_number

Converts a string to a number. Useful when your JSON contains numbers as strings.

**Syntax:**
```liquid
{{ string | to_number }}
```

**Examples:**

```liquid
<!-- Convert string to number for calculations -->
{% assign price = "99.99" | to_number %}
{% assign quantity = "3" | to_number %}
{% assign total = price | times: quantity %}
{{ total }}
<!-- Result: 299.97 -->

<!-- Use with format_number -->
{{ "1234.5" | to_number | format_number: "C" }}
<!-- Result: $1,234.50 -->
```

---

## Tags

Tags provide logic and control flow. They use `{% %}` syntax.

### secUp

Increments and displays a section counter. The counter only increments when the tag is actually rendered (useful with conditional content).

**Syntax:**
```liquid
{% secUp counterName %}
```

**Example:**

```liquid
<h2>{% secUp mainSection %}. Introduction</h2>
<!-- Output: 1. Introduction -->

{% if showMethods %}
<h2>{% secUp mainSection %}. Methods</h2>
<!-- Output: 2. Methods (only if showMethods is true) -->
{% endif %}

<h2>{% secUp mainSection %}. Conclusion</h2>
<!-- Output: 2. Conclusion (if Methods was skipped) -->
<!-- Output: 3. Conclusion (if Methods was shown) -->
```

!!! tip "Counter Names"
    You can use any name for your counter. Use different names for different numbering sequences (e.g., `mainSection`, `subSection`, `figure`).

---

### secCt

Returns the current value of a section counter without incrementing it.

**Syntax:**
```liquid
{% secCt counterName %}
```

**Example:**

```liquid
<h2>{% secUp mainSection %}. Introduction</h2>
<!-- Output: 1. Introduction -->

<h3>{% secCt mainSection %}.1 Background</h3>
<!-- Output: 1.1 Background -->

<h3>{% secCt mainSection %}.2 Objectives</h3>
<!-- Output: 1.2 Objectives -->

<h2>{% secUp mainSection %}. Methods</h2>
<!-- Output: 2. Methods -->
```

---

## Combining Filters

Chain multiple filters together:

```liquid
<!-- Parse date, then format it -->
{{ order.date | parse_date | date: "%B %d, %Y" }}

<!-- Convert to number, multiply, then format as currency -->
{{ item.price | to_number | times: item.quantity | format_number: "C" }}
```

## See Also

- [Liquid Reference](liquid-reference.md) - Standard Liquid filters and tags
- [Localization](localization.md) - Culture settings for number and currency formatting
- [Variables](variables.md) - Using variables in templates
