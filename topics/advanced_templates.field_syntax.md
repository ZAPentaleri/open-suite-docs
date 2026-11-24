# Advanced template field syntax
<!-- id: topic:advanced_templates.field_syntax -->

Part of: [Advanced PDF/HTML templates](advanced_templates.md)

See also: [catalog:template_format_methods](../gen/catalogs/template_format_methods.md),
[Advanced template markup and BFO](advanced_templates.bfo.md).

Data reaches a template through FreeMarker interpolations, written `${...}`. Four kinds of field can
be printed: body fields, sublist fields, fields on related records and saved search result fields. A
template is not a script: SuiteScript APIs cannot be called from inside it.

## Data model objects

The source names these top-level objects: `companyInformation`, `record`, `records`, `customer`,
`salesorder`, `preferences` and `user`.

- `companyInformation`, `preferences` and `user` are available in every template.
- Statements additionally get a `customer` object.
- The source says nothing further about `records` or `salesorder`.

`companyInformation` carries URL-related fields: `companyId`, `appDomain`, `customerCenterDomain`,
`customerLoginUrl` and `extformsDomain`.

## Body fields

A body field is written `${object.fieldId}`, for example `${record.memo}`. A field the editor's field
selector does not offer (the entity's email on the record is one such case) can still be typed in by
hand.

## Related records

To read a field on the record a select field points to, chain the IDs:
`${record.related_field.fieldId}`. This works only through select fields.

> [!SOURCE-ERROR] The record types in the source's related-record example do not agree with each
> other. Rely on the pattern, and check that the select field you follow really points to the record
> type whose field you print.
<!-- source-error: topic:advanced_templates.field_syntax | - | related_record_example_mismatched_record_types -->

## Field suffixes

| Suffix | What it gives |
|---|---|
| `@label` | The field's label |
| `@url` | The link as a bare URL, without anchor markup |
| `@title` | The name of the record type |

On an object that is not a record, `@title` comes out blank.

> [!SOURCE-ERROR] The source's hyperlink example confuses the `href` and `src` attributes and is
> malformed. Put the `@url` value into the attribute the element actually uses, and check the
> generated markup in a preview.
<!-- source-error: topic:advanced_templates.field_syntax | - | hyperlink_markup_example_malformed -->

## Sublists

- Loop over a sublist with `<#list record.sublist as line>` and read its columns from the loop
  variable, for example `${line.quantity}`.
- Sublist indexes, item indexes and the FreeMarker list index all start at 0.
- When a sublist has the same ID as a field, the field selector shows the sublist with the suffix
  `@list`, and that is how you address it.
- A sublist nested in a sublist (second level) cannot be referenced.
- Subrecords are not supported, except for the inventory detail list. Printing inventory detail lists
  every bin and every serial number.
- To print lines in a different order, use FreeMarker's `sort_by` built-in.

## Hidden fields and booleans

A visible sublist checkbox arrives as a real boolean, but a hidden sublist field arrives as a string
holding `T` or `F`. Comparing a boolean with a string raises a template error, so test the value with
`?is_boolean` or `?is_string` first and compare accordingly.

> [!SOURCE-ERROR] The source's example of this check misspells the field ID as `corparatecreditcard`
> [sic]. Take field IDs from the record definition, not from that example.
<!-- source-error: topic:advanced_templates.field_syntax | - | example_misspells_field_id -->

## Address fields

Address parts are written `${record.addressfield.field}`. Parts the source gives as examples:
`addressee`, `addr1`, `addr2`, `city`, `dropdownstate`, `zip`, `country` and `addrphone`.

> [!SOURCE-ERROR] In one place the source spells the addressee part `addresee` [sic]. The list above
> uses `addressee`; if nothing prints, check which spelling your account accepts.
<!-- source-error: topic:advanced_templates.field_syntax | - | address_field_spelled_addresee -->

## Saved search results

A result field is written `${results[index].join.fieldId}`. Result indexes start at 0.

## Multi-currency statements

Turning a statement template into the multi-currency form takes two steps:

1. Wrap the content in a `pdfset` that lists over the statements.
2. Change references to the record's items into references to the record's lines.

## Formatting computed values

As soon as you do arithmetic on a field, the result is a plain FreeMarker value and NetSuite's
formatting is gone. The `nsformat_*` functions, listed in
[catalog:template_format_methods](../gen/catalogs/template_format_methods.md), print values you
compute in the same format NetSuite uses for its standard fields.

- An empty or null input yields an empty string.
- Double-precision numbers are accepted.

Arguments by position, as the source lists them. A type written `a|b` means the argument may be
either type.

| Function | Argument 1 | Argument 2 | Example of argument 2 |
|---|---|---|---|
| `nsformat_date` | `date\|string` | | |
| `nsformat_datetime` | `date\|string` | | |
| `nsformat_time` | `date\|string` | | |
| `nsformat_currency` | `number\|string` | `string` (optional) | `EUR` |
| `nsformat_rate` (one-argument form) | `boolean\|string\|number` | | |
| `nsformat_rate` (two-argument form) | `number\|string` | `string` | `USD` |
| `nsformat_number` | `number\|string` | locale string (optional) | `cs_CZ` |
| `nsformat_boolean` | `boolean\|string\|number` | | |
| `nsformat_password` | `string` | | |
| `nsformat_percent` | `number\|string` | | |
| `nsformat_email` | `string` | | |
| `nsformat_url` | `string` | | |

The source does not state what the second argument of `nsformat_currency` and `nsformat_rate` means
beyond the examples, which are currency codes.

> [!SOURCE-ERROR] For the one-argument form of `nsformat_rate`, the source gives exactly the argument
> types of `nsformat_boolean` (boolean, string or number), which looks copied from that entry rather
> than describing a rate. Prefer passing a number or numeric string, and check the printed result in
> a preview before relying on the one-argument form.
<!-- source-error: topic:advanced_templates.field_syntax | - | nsformat_rate_one_arg_types_match_nsformat_boolean -->

> [!SOURCE-ERROR] The source's number-formatting examples have unbalanced quotes, and the outputs they
> show do not follow from their inputs. Do not copy them; check the actual output in a preview.
<!-- source-error: topic:advanced_templates.field_syntax | - | number_examples_unbalanced_quotes_and_output_mismatch -->

> [!SOURCE-ERROR] Where the source shows the two-argument signature of `nsformat_number`, a closing
> brace sits outside the code formatting, so the signature as printed is broken. Rely on the argument
> list in the table above and verify the call in a preview.
<!-- source-error: topic:advanced_templates.field_syntax | - | nsformat_number_two_arg_signature_brace_outside_code -->

## File Cabinet content

The `@filecabinet` directive embeds text, images or fonts stored in the File Cabinet. It is written
in the form `<@filecabinet/>` and takes two attributes:

- `nstype`: the kind of content, for example `image`;
- `src`: the File Cabinet media URL of the file.

For example `<@filecabinet nstype="image" src="..."/>`, with the file's media URL in `src`. Ticking
the `"File Cabinet Item"` checkbox in the editor writes the directive for you.

## Example

A sales order packing fragment written for this page. Lines print in quantity order, a hidden custom
column is tested safely whatever type it arrives as, a computed line total is formatted like a
standard amount, and the ship-to city and a field label come from the record.
`custcol_fragile_pack` is an invented column ID.

```ftl
<p>${record.memo@label}: ${record.memo}</p>
<p>Ship to: ${record.shippingaddress.city}, ${record.shippingaddress.zip}</p>
<table>
<#list record.item?sort_by("quantity") as line>
  <#assign fragile = line.custcol_fragile_pack>
  <#assign isFragile = (fragile?is_boolean && fragile) || (fragile?is_string && fragile == "T")>
  <tr>
    <td>${line?index + 1}</td>
    <td>${line.item}</td>
    <td>${line.quantity}</td>
    <td>${nsformat_currency(line.quantity * line.rate)}</td>
    <td><#if isFragile>Handle with care</#if></td>
  </tr>
</#list>
</table>
```
