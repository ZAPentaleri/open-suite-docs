# Scriptable email templates
<!-- id: topic:scriptable_templates -->

Sub-topics:

- [Scriptable template synthetic fields: notifications](scriptable_templates.synthetic_fields_notifications.md)
- [Scriptable template synthetic fields: records](scriptable_templates.synthetic_fields_records.md)

See also: [Advanced PDF/HTML templates](advanced_templates.md).

Scriptable templates are email and fax templates written in HTML combined with FreeMarker. They take
the place of the CRMSDK tags that version 1 templates used.

## Uses

- marketing campaign email;
- email and fax templates;
- bulk merge;
- system email;
- email sent from a record's communication;
- transaction messages.

To view these templates a user needs the email template view permission. A template file may be a
`.txt`, `.html` or `.htm` file.

For system templates, the scriptable version is the one whose name ends in "2.0".

## Records each template type can use

| Template type | Records available |
|---|---|
| Campaign email | Entity, parent company, campaign event |
| Case notification | Case, customer |
| Customer issue | Issue, case |
| Employee issue notification | Issue |
| Online form | Customer, online form |
| Login access notification | Entity |
| Subscriptions opt-in | Entity, subsidiary |
| Website digital delivery | Website, customer, sales order |
| Website order | Website, customer, sales order |
| Website other | Website, customer |

## FreeMarker in these templates

- The data is made of three node kinds: scalars, hashes and sequences. Sequences are indexed from 0.
- Field and sublist IDs are the ones SuiteScript uses; you may write them in camel case or entirely in
  lower case.
- A body field is written `${record.field}`. A sublist is iterated with
  `<#list record.sublist as line>`.
- Before you read a sublist row by its index, guard the access with a `has_content` check.
- Markup that sits inside a FreeMarker instruction must be enclosed between `<#--FM:BEGIN-->` and
  `<#--FM:END-->`.
- The sender's signature is available as `preferences.message_signature`.
- Image fields you can print are `storeDisplayImage`, `storeDisplayThumbnail` and `image`.

> [!SOURCE-ERROR] The source shows the custom record field syntax without the leading dollar sign,
> so it would come out as literal text. Write custom record fields with the full `${...}` form like
> every other field.
<!-- source-error: topic:scriptable_templates | - | custom_record_syntax_shown_without_dollar -->

- Synthetic fields exist only inside templates; SuiteScript cannot read them. The two sub-topics list
  them.

## Including files

- Include another file with `<#include "file_id_or_url">`, giving either a File Cabinet file ID or a
  URL. A template ID is not accepted.
- If fetching an external include takes longer than 5 seconds, it is skipped.
- Includes over SSL need a trusted certificate.
- An included file cannot itself contain include directives.

> [!SOURCE-ERROR] The source's link text talks about an "insert" directive, while its code uses the
> include directive. The facts here are about include; treat the "insert" wording as a slip.
<!-- source-error: topic:scriptable_templates | - | file_link_text_says_insert_directive_code_uses_include -->

## Samples

The source offers three samples: an issue notification, a marketing email that promotes an upsell,
and a web store order email. They are not reproduced here.

> [!SOURCE-ERROR] The `td` tags in the source's issue notification sample are malformed. If you
> build on that sample, repair the table markup so that every cell is opened and closed correctly.
<!-- source-error: topic:scriptable_templates | - | issue_sample_has_malformed_td_tags -->

## Converting CRMSDK templates

- A CRMSDK template can be converted into a scriptable template. The result keeps the same ID and
  has "converted" appended to its name.
- Once converted, update the scripts and workflows that use the template.
- These CRMSDK tags were removed and are not carried over: `NLISSUEDUPLICATEOFURL`,
  `NLISSUEDUPLICATEOF` and `NLAVILABLEONLINE` [sic].

> [!SOURCE-ERROR] The removed tag `NLAVILABLEONLINE` [sic] may be a misspelling of "available". The
> source does not confirm the correct spelling, so search legacy templates for both forms.
<!-- source-error: topic:scriptable_templates | - | removed_tag_spelled_nlavilableonline_possible_typo -->

## Example

A case follow-up written for this page. The first issue linked to the case is printed only when the
case has any, and a table built inside an `#if` is enclosed in the markup markers.

```ftl
<p>Your case ${record.casenumber} has been updated.</p>
<#if record.issues?has_content>
<#--FM:BEGIN-->
<table>
  <tr><td>Related issue</td><td>${record.issues[0].issuenumber}</td></tr>
</table>
<#--FM:END-->
</#if>
<p>${preferences.message_signature}</p>
```

`casenumber` and `issuenumber` are used here for illustration; take the actual field IDs from the
record definitions in your account.
