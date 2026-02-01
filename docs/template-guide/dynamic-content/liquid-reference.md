# Liquid Reference

TemplateTo uses the [Liquid](https://shopify.github.io/liquid/) templating language. This page provides a quick reference for common syntax.

## Output

Insert values using double curly braces:

```liquid
{{ variable }}
{{ object.property }}
{{ array[0] }}
```

### Filters

Transform values with filters (pipe syntax):

```liquid
{{ name | upcase }}
{{ price | round: 2 }}
{{ text | truncate: 50 }}
{{ items | size }}
```

Chain multiple filters:

```liquid
{{ name | downcase | capitalize }}
{{ date | parse_date | date: "%Y-%m-%d" }}
```

## Tags

Tags provide logic. They don't output anything directly.

```liquid
{% if condition %}...{% endif %}
{% for item in array %}...{% endfor %}
{% assign variable = value %}
```

## Control Flow

### if / elsif / else

```liquid
{% if customer.tier == "gold" %}
    <span class="gold-badge">Gold Member</span>
{% elsif customer.tier == "silver" %}
    <span class="silver-badge">Silver Member</span>
{% else %}
    <span>Standard Member</span>
{% endif %}
```

### unless

The opposite of `if`:

```liquid
{% unless order.shipped %}
    <p>Order is being processed</p>
{% endunless %}
```

### case / when

```liquid
{% case status %}
    {% when "pending" %}
        <span class="yellow">Pending</span>
    {% when "approved" %}
        <span class="green">Approved</span>
    {% when "rejected" %}
        <span class="red">Rejected</span>
    {% else %}
        <span>Unknown</span>
{% endcase %}
```

## Operators

### Comparison

| Operator | Description |
|----------|-------------|
| `==` | Equal |
| `!=` | Not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater or equal |
| `<=` | Less or equal |

```liquid
{% if price > 100 %}...{% endif %}
{% if status != "cancelled" %}...{% endif %}
```

### Logical

| Operator | Description |
|----------|-------------|
| `and` | Both conditions true |
| `or` | Either condition true |

```liquid
{% if quantity > 0 and inStock %}
    Available
{% endif %}

{% if isAdmin or isModerator %}
    Edit link
{% endif %}
```

### Contains

Check if string contains substring, or array contains value:

```liquid
{% if email contains "@company.com" %}
    Internal user
{% endif %}

{% if tags contains "urgent" %}
    <span class="urgent">!</span>
{% endif %}
```

## Iteration

### for loop

```liquid
{% for product in products %}
    <div>{{ product.name }}</div>
{% endfor %}
```

### Loop parameters

```liquid
<!-- Limit to 5 items -->
{% for item in items limit:5 %}

<!-- Skip first 2 items -->
{% for item in items offset:2 %}

<!-- Reverse order -->
{% for item in items reversed %}
```

### forloop object

```liquid
{% for item in items %}
    {{ forloop.index }}     <!-- 1, 2, 3... -->
    {{ forloop.index0 }}    <!-- 0, 1, 2... -->
    {{ forloop.first }}     <!-- true on first -->
    {{ forloop.last }}      <!-- true on last -->
    {{ forloop.length }}    <!-- total items -->
{% endfor %}
```

### Range loops

```liquid
{% for i in (1..5) %}
    {{ i }}
{% endfor %}
<!-- Output: 1 2 3 4 5 -->
```

### Empty fallback

```liquid
{% for item in items %}
    {{ item.name }}
{% else %}
    No items found.
{% endfor %}
```

### break and continue

```liquid
{% for item in items %}
    {% if item.hidden %}
        {% continue %}
    {% endif %}

    {{ item.name }}

    {% if forloop.index > 10 %}
        {% break %}
    {% endif %}
{% endfor %}
```

## Variables

### assign

Create a variable:

```liquid
{% assign greeting = "Hello" %}
{% assign fullName = firstName | append: " " | append: lastName %}

{{ greeting }}, {{ fullName }}!
```

### capture

Capture a block of content into a variable:

```liquid
{% capture address %}
{{ street }}
{{ city }}, {{ state }} {{ zip }}
{% endcapture %}

<pre>{{ address }}</pre>
```

## Common Filters

### String Filters

| Filter | Example | Result |
|--------|---------|--------|
| `upcase` | `{{ "hello" \| upcase }}` | HELLO |
| `downcase` | `{{ "HELLO" \| downcase }}` | hello |
| `capitalize` | `{{ "hello world" \| capitalize }}` | Hello world |
| `strip` | `{{ "  hello  " \| strip }}` | hello |
| `truncate` | `{{ "Hello world" \| truncate: 8 }}` | Hello... |
| `replace` | `{{ "hello" \| replace: "e", "a" }}` | hallo |
| `split` | `{{ "a,b,c" \| split: "," }}` | ["a","b","c"] |
| `append` | `{{ "hello" \| append: " world" }}` | hello world |
| `prepend` | `{{ "world" \| prepend: "hello " }}` | hello world |

### Number Filters

| Filter | Example | Result |
|--------|---------|--------|
| `plus` | `{{ 4 \| plus: 2 }}` | 6 |
| `minus` | `{{ 4 \| minus: 2 }}` | 2 |
| `times` | `{{ 4 \| times: 2 }}` | 8 |
| `divided_by` | `{{ 10 \| divided_by: 3 }}` | 3 |
| `modulo` | `{{ 10 \| modulo: 3 }}` | 1 |
| `round` | `{{ 4.6 \| round }}` | 5 |
| `floor` | `{{ 4.6 \| floor }}` | 4 |
| `ceil` | `{{ 4.2 \| ceil }}` | 5 |
| `abs` | `{{ -5 \| abs }}` | 5 |

### Array Filters

| Filter | Description |
|--------|-------------|
| `size` | Number of items |
| `first` | First item |
| `last` | Last item |
| `join` | Join items with separator |
| `sort` | Sort items |
| `reverse` | Reverse order |
| `uniq` | Remove duplicates |
| `map` | Extract property from each item |
| `where` | Filter by property |
| `compact` | Remove nil values |

```liquid
{{ items | size }}
<!-- Result: 5 -->

{{ items | map: "name" | join: ", " }}
<!-- Result: Item 1, Item 2, Item 3 -->

{{ items | where: "active", true | size }}
<!-- Result: 3 (items where active is true) -->

{{ items | sort: "price" | first | json }}
<!-- Cheapest item -->
```

### Date Filters

```liquid
{{ "now" | date: "%Y-%m-%d" }}
<!-- Current date: 2024-01-15 -->

{{ order.date | date: "%B %d, %Y" }}
<!-- January 15, 2024 -->
```

!!! note "TemplateTo Extension"
    Use `parse_date` to convert date strings before using the `date` filter. See [Filters & Tags](filters-tags.md).

## TemplateTo Extensions

Beyond standard Liquid, TemplateTo provides:

- **Filters**: `parse_date`, `format_number`, `to_number` - See [Filters & Tags](filters-tags.md)
- **Tags**: `secUp`, `secCt` - See [Filters & Tags](filters-tags.md)
- **Functions**: `repeat()`, `qrCode()`, `barcode()`, `contentBlock()`, `isArray()` - See [Functions](functions/index.md)

## External Resources

- [Official Liquid Documentation](https://shopify.github.io/liquid/)
- [Liquid Cheat Sheet](https://www.shopify.com/partners/shopify-cheat-sheet)
