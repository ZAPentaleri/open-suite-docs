# Advanced PDF/HTML templates
<!-- id: topic:advanced_templates -->

Sub-topics:

- [Advanced template markup and BFO](advanced_templates.bfo.md)
- [Advanced template field syntax](advanced_templates.field_syntax.md)
- [Scripting advanced templates](advanced_templates.scripting.md)
- [Advanced template fonts, languages and bar codes](advanced_templates.localization_and_barcodes.md)

See also: [Scriptable email templates](scriptable_templates.md), [N/render](../modules/N_render.md),
[catalog:template_format_methods](../gen/catalogs/template_format_methods.md),
[catalog:template_fonts](../gen/catalogs/template_fonts.md),
[catalog:template_barcode_types](../gen/catalogs/template_barcode_types.md).

Advanced PDF/HTML templates decide what a record or transaction looks like when it is printed. The
template language is FreeMarker, and a single template covers both outputs: the same file yields the
PDF version and the HTML version of the document. Driving templates from SuiteScript is covered in
[Scripting advanced templates](advanced_templates.scripting.md).

## Prerequisites

- The account needs the feature `"Advanced PDF/HTML Templates"` enabled. Most accounts already have
  it on.
- The user needs the permission `"Advanced PDF/HTML Templates"` (the permission carries the same name
  as the feature).
- A custom form offers two printing types, basic and advanced. A form that uses advanced templates
  can be switched back to the basic layouts.
- The basic PDF and HTML layouts are deprecated and scheduled for removal; forms still on them should
  be moved to advanced templates.

## Assigning templates

- Templates are attached to a custom form, one per print type. Every print type the basic layouts
  handled can use an advanced template, and internationalized versions are supported as well.
- A custom form can name one template for printing and a different one for email.
- You do not need separate PDF and HTML templates: one template produces both.
- Leaving the template choice at its default hands printing over to the browser's own print
  function.

## Third-party components

| Component | Version |
|---|---|
| FreeMarker (template language) | 2.3.26 |
| BFO Report Generator (PDF engine) | 1.2.10 |
| TinyMCE (WYSIWYG editor) | 7.5.1 |

## How a template is rendered

Printing runs in three stages: FreeMarker processes the template, BFO validates the XML that comes
out, and BFO then builds the PDF from that XML. The XML passed to PDF creation may be at most 100 MB.

- Prefer PDF output; the editor cannot preview HTML output.
- When the printing preference that prints using HTML is set, printing can fail with an invalid PDF
  layout error.
- Avoid tables nested inside tables.
- The `cellspacing` attribute is not supported; use `cellmargin` instead. A body width expressed as a
  percentage is not supported either; give an absolute width.
- Line-break markup inside a field value is printed as literal text instead of breaking the line.

The markup rules are on [Advanced template markup and BFO](advanced_templates.bfo.md).

## Editing templates

- The editor has two modes, WYSIWYG and source code. Bar codes and sublists can be added only in
  source code mode.
- Switching back to WYSIWYG after editing the source can lose part of the content.
- A template can have only one WYSIWYG header.
- Every File Cabinet file the template refers to must either be available without login or be a
  company-wide file. A company-wide file has to be pulled in with the `@filecabinet` directive (see
  [field syntax](advanced_templates.field_syntax.md#file-cabinet-content)).
- Put your account-specific domain into URLs inside templates. URLs that use a data-center domain are
  rewritten at print time, but the stored template is left unchanged.

## Managing templates across accounts

- Templates can be moved between accounts with SDF or with Copy to Account.
- Script IDs of custom templates begin with `custtmpl`. A standard template's script ID cannot be
  changed.

## Printing preferences

These printing preferences still apply when a form prints with an advanced template:

- customers default to print;
- vendors default to print;
- the return form is printed together with packing slips;
- the remittance form is printed with invoices and statements;
- transaction forms in landscape;
- separate vouchers for bill payments;
- accounts printed on vouchers;
- discount and shipping lines shown in columns;
- the location address printed on forms;
- the default check type.

The source describes these by meaning only and does not state their exact UI labels. The message
texts that printing preferences let you enter are not applied; put any such message into the
template itself.

## Statements in several currencies

- A standard multi-currency statement template exists.
- When multiple currencies are enabled in the account, the multi-currency statement is used; an
  account with a single currency uses the standard statement template.
- A multi-currency statement loops over a list of statements instead of taking one record as its
  root. The conversion steps are on
  [Advanced template field syntax](advanced_templates.field_syntax.md#multi-currency-statements).
- When a statement is sent with Save & Email, the attachment is rendered in the customer's language.

## Formatting tips

- To print a month name in capitals, apply FreeMarker's `upper_case` built-in to it.

> [!SOURCE-ERROR] The source's upper-case example puts the dollar sign inside the braces, which is not
> a FreeMarker interpolation. An interpolation is the dollar sign followed by braces, `${...}`.
<!-- source-error: topic:advanced_templates | - | upper_case_example_puts_dollar_inside_braces -->

- Hidden sublist fields arrive as text rather than booleans, so check the type before comparing (see
  [field syntax](advanced_templates.field_syntax.md#hidden-fields-and-booleans)).

> [!SOURCE-ERROR] The source's hidden-field example writes the logical OR as two pipe characters with
> a space between them, which does not parse. Write the OR operator as two adjacent pipes.
<!-- source-error: topic:advanced_templates | - | hidden_field_example_has_broken_or_operator -->

## Printing saved search results

- An advanced template can print the results of a saved search on any record type, and a search can
  have several templates.
- More than ten result columns make the printout hard to read.
- The internal ID can be printed only when it is one of the search's result columns.
- Where you place the list directive controls pagination: wrapping the whole page content in it gives
  each result record a page of its own.
- When a search has several currency formula columns, the result variable exposes them as
  `result.formulacurrency` (first), `result.formulacurrency_1` (second) and
  `result.formulacurrency_2` (third), following the column order.

> [!SOURCE-ERROR] The source's second formula-column example has no dollar sign, so it would print as
> literal text instead of the value. Every field reference needs the full `${...}` form.
<!-- source-error: topic:advanced_templates | - | second_formula_example_lacks_dollar_sign -->

## Example

A saved search template fragment written for this page. It assumes a search with an entity column and
two currency formula columns (say, an open balance and an overdue balance); the name of the entity
column depends on your search.

```ftl
<table>
  <tr>
    <th>Vendor</th><th align="right">Open</th><th align="right">Overdue</th>
  </tr>
  <#list results as result>
  <tr>
    <td>${result.entity}</td>
    <td align="right">${result.formulacurrency}</td>
    <td align="right">${result.formulacurrency_1}</td>
  </tr>
  </#list>
</table>
```
