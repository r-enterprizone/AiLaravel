---
paths:
  - "app/**"
  - "resources/views/**"
---

# Arabic Percent Sign Rendering (RTL BiDi)

Never concatenate `.'٪'` (or `%'`) directly after a number in display strings.
Always use the `arabic_percent()` helper from `app/Support/helpers.php`:

```php
arabic_percent($rate);   // بدل: $rate.'٪'
{{ arabic_percent($tier->charity_rate) }}   // بدل: {{ $tier->charity_rate }}٪
```

The helper inserts an invisible LRM (U+200E) between the digits and the sign.
Without it the BiDi algorithm glues the percent sign (ET) to western digits (EN),
so an RTL reader hits the sign *before* the number; with the mark the sign
renders to the left of the digits and is read last.

A standalone sign with no number (e.g. a field label like «نسبة الدعم ٪») needs
no helper. Tables that pattern-match numeric cells must include `\x{200E}` with
the `u` flag in their regex (see report-show.blade.php).
