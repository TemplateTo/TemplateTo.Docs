# Conditional Content

Show or hide sections of your template based on data values. These patterns help create flexible templates that adapt to different scenarios.

## Basic If Statement

Show content only when a condition is true:

```liquid
{% if customer.isPremium %}
    <div class="premium-badge">Premium Member</div>
{% endif %}
```

## If/Else

Handle both cases:

```liquid
{% if order.shipped %}
    <p>Your order has shipped!</p>
{% else %}
    <p>Your order is being processed.</p>
{% endif %}
```

## Multiple Conditions (elsif)

Check several possibilities:

```liquid
{% if order.status == "delivered" %}
    <span class="status delivered">Delivered</span>
{% elsif order.status == "shipped" %}
    <span class="status shipped">In Transit</span>
{% elsif order.status == "processing" %}
    <span class="status processing">Processing</span>
{% else %}
    <span class="status pending">Pending</span>
{% endif %}
```

## Checking for Values

### Exists and Not Empty

```liquid
<!-- Check if property exists and has a value -->
{% if customer.phone %}
    <p>Phone: {{ customer.phone }}</p>
{% endif %}

<!-- Check for non-blank string -->
{% if customer.notes != blank %}
    <div class="notes">{{ customer.notes }}</div>
{% endif %}
```

### Checking for Null/Empty

```liquid
{% if address == nil %}
    <p>No address on file</p>
{% endif %}

{% if items == empty %}
    <p>No items in cart</p>
{% endif %}

{% if items.size == 0 %}
    <p>Your cart is empty</p>
{% endif %}
```

## Comparing Values

### Equality

```liquid
{% if country == "UK" %}
    <p>Free shipping within the UK!</p>
{% endif %}

{% if quantity == 1 %}
    <span>item</span>
{% else %}
    <span>items</span>
{% endif %}
```

### Numeric Comparisons

```liquid
{% if order.total > 100 %}
    <p class="free-shipping">Qualifies for free shipping!</p>
{% endif %}

{% if stock <= 5 %}
    <span class="low-stock">Only {{ stock }} left!</span>
{% endif %}

{% if age >= 18 %}
    <p>Adult content accessible</p>
{% endif %}
```

## Logical Operators

### And (both must be true)

```liquid
{% if user.verified and user.active %}
    <span class="verified-badge">Verified User</span>
{% endif %}

{% if quantity > 0 and inStock %}
    <button>Add to Cart</button>
{% endif %}
```

### Or (either can be true)

```liquid
{% if isAdmin or isModerator %}
    <a href="/admin">Admin Panel</a>
{% endif %}

{% if paymentMethod == "credit" or paymentMethod == "debit" %}
    <p>Card payment selected</p>
{% endif %}
```

### Combining Operators

```liquid
{% if isPremium and (orderTotal > 50 or hasPromoCode) %}
    <p>You qualify for express shipping!</p>
{% endif %}
```

## Contains

Check if a string contains a substring:

```liquid
{% if email contains "@company.com" %}
    <span>Internal User</span>
{% endif %}

{% if product.tags contains "sale" %}
    <span class="sale-badge">On Sale!</span>
{% endif %}
```

## Unless (Negative Condition)

The opposite of `if`:

```liquid
{% unless order.paid %}
    <div class="payment-reminder">
        Payment is still pending.
    </div>
{% endunless %}

<!-- Equivalent to: -->
{% if order.paid == false %}
    ...
{% endif %}
```

## Case/When (Switch)

For multiple specific values:

```liquid
{% case shipping.method %}
    {% when "standard" %}
        <p>Delivery in 5-7 business days</p>
    {% when "express" %}
        <p>Delivery in 2-3 business days</p>
    {% when "overnight" %}
        <p>Next business day delivery</p>
    {% else %}
        <p>Delivery time varies</p>
{% endcase %}
```

## Type Checking with isArray()

Handle both single items and arrays:

```liquid
{% if isArray(recipients) %}
    <h3>Recipients:</h3>
    <ul>
        {% for person in recipients %}
            <li>{{ person.name }}</li>
        {% endfor %}
    </ul>
{% else %}
    <p>Recipient: {{ recipients.name }}</p>
{% endif %}
```

## Optional Sections

### Show section if data exists

```liquid
{% if order.notes %}
<div class="order-notes">
    <h3>Special Instructions</h3>
    <p>{{ order.notes }}</p>
</div>
{% endif %}
```

### Multiple optional fields

```liquid
{% if customer.phone or customer.mobile %}
<div class="contact-info">
    {% if customer.phone %}
        <p>Phone: {{ customer.phone }}</p>
    {% endif %}
    {% if customer.mobile %}
        <p>Mobile: {{ customer.mobile }}</p>
    {% endif %}
</div>
{% endif %}
```

## Conditional Styling

### Inline classes

```liquid
<div class="order-status {% if order.urgent %}urgent{% endif %}">
    {{ order.status }}
</div>

<tr class="{% if forloop.first %}first-row{% endif %} {% if item.highlighted %}highlight{% endif %}">
```

### Conditional CSS

```liquid
<style>
{% if showBorder %}
    .container { border: 1px solid #000; }
{% endif %}
</style>
```

## Conditional Table Sections

### Optional table columns

```liquid
<table>
    <thead>
        <tr>
            <th>Item</th>
            <th>Quantity</th>
            {% if showDiscount %}
                <th>Discount</th>
            {% endif %}
            <th>Total</th>
        </tr>
    </thead>
    <tbody>
        {% for item in items %}
        <tr>
            <td>{{ item.name }}</td>
            <td>{{ item.quantity }}</td>
            {% if showDiscount %}
                <td>{{ item.discount | format_number: "P" }}</td>
            {% endif %}
            <td>{{ item.total | format_number: "C" }}</td>
        </tr>
        {% endfor %}
    </tbody>
</table>
```

## First/Last Item Handling

```liquid
{% for item in items %}
    {% if forloop.first %}
        <div class="featured">
    {% endif %}

    <p>{{ item.name }}</p>

    {% if forloop.first %}
        </div>
        <h3>Other Items:</h3>
    {% endif %}
{% endfor %}
```

## Conditional Formatting

### Different formats based on value

```liquid
{% if amount < 0 %}
    <span class="negative">({{ amount | abs | format_number: "C" }})</span>
{% else %}
    <span class="positive">{{ amount | format_number: "C" }}</span>
{% endif %}
```

### Date-based conditions

```liquid
{% assign dueDate = invoice.dueDate | parse_date %}
{% assign today = "now" | date: "%s" | to_number %}
{% assign dueDateTimestamp = dueDate | date: "%s" | to_number %}

{% if dueDateTimestamp < today %}
    <span class="overdue">OVERDUE</span>
{% endif %}
```

## Common Patterns

### Show message for empty results

```liquid
{% if searchResults.size > 0 %}
    {% for result in searchResults %}
        <div>{{ result.title }}</div>
    {% endfor %}
{% else %}
    <p>No results found for "{{ searchQuery }}"</p>
{% endif %}
```

### Pluralization

```liquid
<p>
    {{ items.size }}
    {% if items.size == 1 %}item{% else %}items{% endif %}
    in your cart
</p>
```

### Country-specific content

```liquid
{% case customer.country %}
    {% when "US" %}
        <p>Shipping via USPS</p>
    {% when "UK" %}
        <p>Shipping via Royal Mail</p>
    {% when "DE", "FR", "ES" %}
        <p>Shipping via DHL</p>
    {% else %}
        <p>International shipping available</p>
{% endcase %}
```

## Best Practices

1. **Keep conditions simple** - Complex logic is hard to debug
2. **Check for existence first** - `{% if obj %}{% if obj.property %}{% endif %}{% endif %}`
3. **Use meaningful variable names** - `isPremium` is clearer than `p`
4. **Consider default values** - `{{ value | default: "N/A" }}`
5. **Document complex conditions** - Add comments for future reference

## See Also

- [Liquid Reference](../liquid-reference.md) - Complete operator reference
- [Variables](../variables.md) - Working with data
- [Debugging](../debugging.md) - Troubleshooting conditionals
