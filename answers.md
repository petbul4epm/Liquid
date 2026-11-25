# Answer key

<a id="01-greeting"></a>

## Greeting and fallback

The `default` filter supplies `there` when the name is missing. The `escape` filter makes special characters display as text in HTML.

| Case | Expected visible paragraphs in order |
| --- | --- |
| `01-name` | Hi Alex, |
| `01-missing-name` | Hi there, |
| `01-escaped-name` | Hi Sam &amp; &lt;Jo&gt;, |

<a id="02-savings"></a>

## Savings and GBP formatting

Missing and blank savings both suppress the paragraph. The current template shows £0.00 and renders the unformatted string as supplied. Use a formatted display string upstream and apply any zero-savings rule there or explicitly in Liquid.

| Case | Expected visible paragraphs in order |
| --- | --- |
| `02-formatted` | Hi Alex, / You saved £12.20 on M&amp;S. |
| `02-missing` | Hi Alex, |
| `02-blank` | Hi Alex, |
| `02-zero` | Hi Alex, / You saved £0.00 on M&amp;S. |
| `02-unformatted` | Hi Alex, / You saved 12.2 on M&amp;S. |

<a id="03-basket"></a>

## Basket loop and substitutions

A delivered Snacks item anywhere in the array sets the flag. A substituted snack, an empty or missing array, and the string `"false"` leave the flag unset. Validate the boolean in the payload.

| Case | Expected visible paragraphs in order |
| --- | --- |
| `03-match-later` | Hi Alex, / Try M&amp;S Luxury Tea with your snacks. |
| `03-substituted-only` | Hi Alex, |
| `03-empty` | Hi Alex, |
| `03-missing-items` | Hi Alex, |
| `03-string-boolean` | Hi Alex, |

<a id="04-recommendation"></a>

## Recommendation rules and escaping

Both conditions must be true. Product names are escaped before insertion. Stock and approval checks belong in the source that supplies the recommended product.

| Case | Expected visible paragraphs in order |
| --- | --- |
| `04-both-present` | Hi Alex, / Try M&amp;S Luxury Tea with your snacks. |
| `04-no-product` | Hi Alex, |
| `04-no-snack` | Hi Alex, |
| `04-escaped-product` | Hi Alex, / Try Tea &amp; &lt;Biscuits&gt; with your snacks. |

<a id="05-full-render"></a>

## Full email regression

The complete case renders three paragraphs. The minimal case renders only the fallback greeting.

| Case | Expected visible paragraphs in order |
| --- | --- |
| `05-complete` | Hi Alex, / You saved £12.20 on M&amp;S. / Try M&amp;S Luxury Tea with your snacks. |
| `05-minimal` | Hi there, |
