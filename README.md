# Post-shop email Liquid exercises

Practice for the Ocado CRM interview scenario by **petbul4epm**. All test profiles and order data are fictional.

- [Greeting and fallback](exercises/01-greeting.md)
- [Savings and GBP formatting](exercises/02-savings.md)
- [Basket loop and substitutions](exercises/03-basket.md)
- [Recommendation rules and escaping](exercises/04-recommendation.md)
- [Full email regression](exercises/05-full-render.md)

Use the [Liquid template](templates/post-shop-email.html.liquid), the [JSON cases](fixtures/cases.json) and the [answer key](answers.md).

## Run a case in Braze

1. Put the template inside the body of an email in a sandbox API-triggered campaign.
2. Open **Preview and Test**. Use the `profile` object for test user details and the `trigger_properties` object for campaign trigger values. Start with a fresh preview for each case so omitted fields stay missing.
3. The fixture wrapper names are for this worksheet. Copy the contents of `trigger_properties` to the corresponding input. Keep arrays, booleans and strings in their JSON types. For an API test with a dedicated test recipient, pass that object as the recipient’s trigger properties and set up the first name on the test profile.
4. Compare the rendered text and any specified HTML fragments with the case's `expected` values. Record pass or fail and what you would check first if it differs.

These are manual test cases with expected results. The Braze-specific placeholders need the Braze campaign context.

[Braze trigger properties documentation](https://braze.com/docs/api/objects_filters/trigger_properties_object/) · [Braze test messages documentation](https://www.braze.com/docs/user_guide/messaging/messaging_fundamentals/sending_test_messages/?tab=email)

## Presentation

[Download the Ocado CRM interview PowerPoint](Ocado_CRM_Presentation.pptx). Slide 8 contains the Liquid used in these exercises.
