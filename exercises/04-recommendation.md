# Recommendation rules and escaping

Test both conditions of the final `if` independently. Confirm that the recommendation appears only when the basket qualifies and a product name is present. Compare the rendered email and HTML source for the product with special characters.

Use these cases in [fixtures/cases.json](../fixtures/cases.json).

- `04-both-present`
- `04-no-product`
- `04-no-snack`
- `04-escaped-product`

## Extra practice

Explain where you would check that the suggested product is approved and in stock.

Check your work in the [answer key](../answers.md#04-recommendation).
