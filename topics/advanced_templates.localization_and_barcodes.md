# Advanced template fonts, languages and bar codes
<!-- id: topic:advanced_templates.localization_and_barcodes -->

Part of: [Advanced PDF/HTML templates](advanced_templates.md)

See also: [catalog:template_fonts](../gen/catalogs/template_fonts.md),
[catalog:template_barcode_types](../gen/catalogs/template_barcode_types.md),
[SDF translation collections](sdf.translation_collections.md),
[N/translation](../modules/N_translation.md) (the script-side counterpart of the template functions
below).

## Choosing the print language

- The locale is taken from the customer first; if the customer has none, from the subsidiary; failing
  that, from the company.
- A language set for the Customer Center wins over the contact's language.
- Each print uses exactly one locale. To mix languages in one document, mark the passages with the
  `lang` attribute.

## Fonts

- [catalog:template_fonts](../gen/catalogs/template_fonts.md) lists the available fonts and the
  locale each belongs to.
- Templates reach these fonts through `nsfont` variables, written `${nsfont.Font_Style}` (for
  example `${nsfont.NotoSans_Regular}`) in the `src` attribute of a `link` element.
- If characters are missing from the output, set `subset` to false for that font.
- For locales that write numbers with their own digits, link the matching symbol fonts as well.

> [!SOURCE-ERROR] The source writes the Bengali locale as `bn_DB` [sic], where `bn_BD` would be
> expected; the fonts catalog keeps the source's form. Verify which code your account accepts.
<!-- source-error: topic:advanced_templates.localization_and_barcodes | - | bengali_locale_written_bn_db -->

> [!SOURCE-ERROR] One font name in the source contains a stray space. The catalog lists it without
> the space, as `NotoSansCJKsc_Bold`; use that form and check that the font loads.
<!-- source-error: topic:advanced_templates.localization_and_barcodes | - | font_name_has_stray_space -->

### Adding your own font

1. Upload the TrueType (`.ttf`) file to the File Cabinet.
2. Declare the font with subtype `opentype`.
3. Link it through its File Cabinet URL.

## Translated text

Three template functions read translation collections. They work only in source code mode.

| Function | Parameters |
|---|---|
| `nstranslation.load` | `collections`; `locales` (optional) |
| `nstranslation.get` | `collection`; `key`; `locale` (optional) |
| `nstranslation.selectLocale` | `handle`; `locale` |

- Each function takes one hash literal whose keys are the parameter names above, for example
  `nstranslation.load({"collections": [...]})`.
- Each entry in the `collections` list of `nstranslation.load` is itself a hash with the keys
  `alias`, `collection` and `keys`.
- `nstranslation.load` returns a handle. A translated string is then read as `handle.alias.KEY`.
- The first locale loaded becomes the default. When the locale argument is omitted, the current
  FreeMarker locale is used.
- Locale constants are available on `nstranslation.Locale`.
- For a translated string with placeholders, call the string with a hash that holds the placeholder
  values in a params array.

> [!SOURCE-ERROR] The source's translation example misspells `nstranslation` and leaves the quote
> around a key unclosed. Spell it `nstranslation` and close every quoted key.
<!-- source-error: topic:advanced_templates.localization_and_barcodes | - | example_misspells_nstranslation -->

> [!SOURCE-ERROR] In the source's placeholder example, a line ends with a stray `>` that belongs to no
> tag. Leave it out when you write the call.
<!-- source-error: topic:advanced_templates.localization_and_barcodes | - | placeholder_example_line_ends_with_stray_angle_bracket -->

## Logos and other layout details

- A common technique prints the subsidiary's logo when there is one and the company logo otherwise.
- On standard forms a logo is at most 200 pixels wide and 60 pixels high.
- A check's apply sublist is not printed by default; add a list over the apply lines to print it.

## Bar codes

- With the feature `"Bar Coding and Item Labels"` enabled, bar codes are printed automatically on:
  bill of materials, cash refund, cash sale, credit memo, invoice, item label, packing slip, picking
  ticket, purchase order, quote, remittance form, return form, return authorization and sales order.
- In your own template, use the `barcode` element. Its attributes are `codetype`, `showtext`,
  `value`, `height` and `width`. `codetype`, `showtext` and `value` are always required; for
  `qrcode`, `height` and `width` are required too.
- The code types are listed in
  [catalog:template_barcode_types](../gen/catalogs/template_barcode_types.md).
- Values may contain ASCII characters only; special characters cause an error.
- The editor does not display bar codes; check them in a preview or a real print.
- These code types print no human-readable text: `aztec`, `deutchepostmatrix` [sic],
  `intelligentmail`, `maxicode`, `pdf417`, `postnet`, `qrcode` and `rm4scc`.

> [!SOURCE-ERROR] The source spells the Deutsche Post code types inconsistently
> (`deutchepostmatrix` [sic] in one place, a `deutschenpost` form elsewhere). The catalog keeps the
> source's spellings; test the exact `codetype` value before relying on it.
<!-- source-error: topic:advanced_templates.localization_and_barcodes | - | deutsche_post_codetypes_spelled_inconsistently -->

## Examples

A bin label fragment written for this page: a linear code with its text printed below it, and a QR
code with the same value, which therefore needs both dimensions. The second paragraph is marked as
German with the `lang` attribute.

```ftl
<table>
  <tr>
    <td><barcode codetype="code39" showtext="true" value="${record.itemid}"/></td>
    <td><barcode codetype="qrcode" showtext="false" value="${record.itemid}" height="45" width="45"/></td>
  </tr>
</table>
<p>Store dry.</p>
<p lang="de">Trocken lagern.</p>
```

Loading a translation collection and printing two of its strings. The collection script ID and its
keys are invented for this page; translation collection script IDs start with `custcollection_`.

```ftl
<#assign tx = nstranslation.load({"collections": [{"alias": "pack", "collection": "custcollection_packing_notes", "keys": ["HANDLE_WITH_CARE", "THANK_YOU"]}]})>
<p>${tx.pack.HANDLE_WITH_CARE}</p>
<p>${tx.pack.THANK_YOU}</p>
```
