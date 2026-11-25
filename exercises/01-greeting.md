# Greeting and fallback

Find the greeting in the template. Predict what the customer will see with a normal name, with no name, and with a name containing `&` and `<`. Check both the rendered email and its HTML source.

Use these cases in [fixtures/cases.json](../fixtures/cases.json).

- `01-name`
- `01-missing-name`
- `01-escaped-name`

## Extra practice

Temporarily remove `escape`, rerun the last case, and explain the difference. Restore it afterwards.

Check your work in the [answer key](../answers.md#01-greeting).
