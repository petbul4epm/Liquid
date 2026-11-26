# Braze Liquid Command Reference

Ocado CRM interview preparation. Examples use fictional data.

## Syntax and data

| Syntax | What it does |
| --- | --- |
| `{{ value }}` | Outputs a value. |
| `{% tag %}` | Runs an instruction. |
| `{{ value \| filter: argument }}` | Transforms a value. Chained filters run from left to right. |
| `{%- … -%}` and `{{- … -}}` | Trim whitespace on the side marked by a hyphen. |
| `{{ item.name }}` and `{{ items[0] }}` | Read an object property or the first element of an array. |
| `{{${first_name}}}` | Reads a standard Braze profile attribute. |
| `{{custom_attribute.${tier}}}` | Reads a custom profile attribute. |
| `{{api_trigger_properties.${items}}}` | Reads the API-triggered campaign payload. |
| `{{event_properties.${order_id}}}` | Reads an available triggering event property. |
| `{{context.${order_id}}}` | Reads Canvas context, including configured entry properties. |
| `{{campaign.${name}}}` and `{{canvas.${name}}}` | Read metadata in supported campaign or Canvas contexts. |

## Variables, conditions and loops

| Command | What it does |
| --- | --- |
| `assign` | Stores a value or a filtered result. |
| `capture`, `endcapture` | Stores rendered output as a string. |
| `if`, `elsif`, `else`, `endif` | Evaluates conditions in order and renders the first matching branch. |
| `unless`, `endunless` | Renders a block when its condition evaluates as false. |
| `case`, `when`, `else`, `endcase` | Selects a branch based on one value. |
| `for`, `else`, `endfor` | Repeats a block. The optional `else` handles zero iterations. |
| `limit`, `offset`, `reversed` | Controls the number of iterations, starting offset or order. |
| `forloop.index`, `.index0`, `.rindex`, `.rindex0`, `.first`, `.last`, `.length` | Provides position and length information for the active loop. |
| `liquid`, `echo` | Runs one instruction per line inside a single tag and prints values from within it. |

Operators include `==`, `!=`, `>`, `<`, `>=`, `<=`, `and`, `or` and `contains`. Use `contains` for substrings or membership in a string array. Liquid evaluates chains of `and` and `or` from right to left. Use nested branches for explicit grouping. Check optional strings against `blank`, missing values with `nil` and empty strings or collections with `empty`. The Boolean `false` and the string `"false"` are different values. An empty string and `0` are truthy. Assign a filtered result before comparing it in a condition.

Additional Shopify Liquid tags to check in the Braze composer before use include `break`, `continue`, `cycle`, `tablerow`, `increment`, `decrement`, `comment` and `raw`. Shopify `render` and `include` depend on host support for named snippets. Use Braze Content Blocks for reusable email modules.

## Filters

| Purpose | Filters |
| --- | --- |
| Fallback and formatting | `default`, `date`, `money`, `number_with_delimiter` |
| Arrays | `first`, `last`, `size`, `slice`, `join`, `map`, `where`, `compact`, `concat`, `reverse`, `sort`, `sort_natural`, `uniq` |
| Arithmetic, bounds and rounding | `plus`, `minus`, `times`, `divided_by`, `modulo`, `abs`, `at_least`, `at_most`, `round`, `ceil`, `floor` |
| Case and concatenation | `capitalize`, `downcase`, `upcase`, `append`, `prepend` |
| String changes | `remove`, `remove_first`, `replace`, `replace_first`, `split`, `strip`, `strip_newlines`, `truncate`, `truncatewords` |
| HTML | `escape`, `strip_html`, `newline_to_br` |
| URLs | `url_encode`, `url_escape`, `url_param_escape` |
| JSON and object access | `json_escape`, `json_parse`, `as_json_string`, `property_accessor` |
| Encoding and signatures | `base64_encode`, `md5`, `sha1`, `sha2`, `hmac_sha1_hex`, `hmac_sha1_base64`, `hmac_sha256`, `hmac_sha256_hex`, `hmac_sha256_base64`, `hmac_sha512` |

```liquid
<p>Hi {{${first_name} | default: "there" | escape}},</p>
{{ 1220 | divided_by: 100.0 | money | prepend: "£" }}
{{ slot_timestamp | date: "%A %d %B" }}
{% assign snacks = items | where: "category", "Snacks" %}
{{ "snacks & tea" | url_encode }}
```

The money example converts pence to pounds and renders £12.20. Use `escape` for HTML text and attribute values. URL-encode query parameter values and use an approved HTTPS booking destination. Braze lists `find_index`, `money_with_currency`, `money_without_currency`, `lstrip` and `rstrip` as unsupported.

## Braze messaging commands

| Command | What it does |
| --- | --- |
| `connected_content` with `:save` | Fetches API data during rendering and stores the response. |
| `abort_message()` | Suppresses the send to one recipient when a required rule fails. |
| `{{content_blocks.${block_name}}}` | Inserts a reusable Braze module. |
| `catalog_items`, `catalog_selection` | Loads records by ID or selection into `items`. Generate the syntax in the composer. |
| `message_extras` | Attaches metadata to supported send events. |
| `random` | Generates a random number. An integer argument sets the exclusive upper bound. |
| `shopping_cart` | Loads cart data for supported eCommerce Canvas use cases. |
| `subscription_group_unsubscribe_url` | Generates a group-specific unsubscribe link. |
| Promotion code personalisation | Inserts a code from a configured list. Braze can consume codes for every list referenced across the branches. |

```liquid
{% connected_content https://example.com/api/delivery :save delivery %}
{% if delivery != nil and delivery.date_label != blank %}
  <p>Check slots for {{ delivery.date_label | escape }}.</p>
{% else %}
  <p>Check delivery times that work for you.</p>
{% endif %}
```

The endpoint is a placeholder. Validate response fields and freshness before making an availability claim. Braze automatically adds `__http_status_code__` for successful 2xx JSON object responses. Use a fallback for optional delivery details. Use `abort_message` when a required send rule fails.

## Post-shop email walkthrough

```liquid
{% assign items = {{api_trigger_properties.${items}}} %}
{% assign has_snacks = false %}
{% for item in items %}
  {% if item.category == "Snacks" and item.substituted == false %}
    {% assign has_snacks = true %}
  {% endif %}
{% endfor %}
```

- Read the savings, basket, recommendation and delivery fields from the trigger payload with `assign`.
- Use `default` for the greeting and `if`/`else` for savings or generic thanks.
- Require both `has_snacks` and a populated product name for the recommendation.
- Require a known slot status and a date for dated delivery copy. Use `else` for generic delivery copy.
- Render the booking CTA when an approved URL is supplied.
- Preview a qualifying basket, a substituted snack, and a payload with missing fields. Also test incorrect Boolean types, special characters, and missing or stale API data.

[Braze Liquid tags](https://www.braze.com/docs/user_guide/messaging/design_and_edit/personalize/liquid/supported_personalization_tags) · [Braze filters](https://www.braze.com/docs/user_guide/messaging/design_and_edit/personalize/liquid/filters) · [Advanced filters](https://www.braze.com/docs/user_guide/messaging/design_and_edit/personalize/liquid/advanced_filters) · [Connected Content](https://www.braze.com/docs/user_guide/messaging/design_and_edit/personalize/connected_content) · [Shopify Liquid reference](https://shopify.github.io/liquid/) · [Post-shop template](templates/post-shop-email.html.liquid) · [Fictional test cases](fixtures/cases.json)
