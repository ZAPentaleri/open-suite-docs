# Custom fields
<!-- id: topic:custom_fields -->

Sub-topics:

- [Custom field defaults and formulas](custom_fields.defaults_formulas.md)
- [Custom field sourcing](custom_fields.sourcing.md)
- [Summary search custom fields](custom_fields.summary_search.md)

See also: [Custom lists](custom_lists.md), [Custom records](custom_records.md),
[Custom segments](custom_segments.md), [N/ui/serverWidget](../modules/N_ui_serverWidget.md),
SDF field type values: [catalog:sdf_list_generic_customfield_fieldtype](../gen/catalogs/sdf_list_generic_customfield_fieldtype.md).

Custom fields add account-specific data points to standard and custom records. Scripts read and write
them like any other field, so the field's type, its size limits and its storage quirks decide what your
code can count on.

## Kinds of custom field

Each custom field is defined for a family of records:

| Kind | Attaches to |
|---|---|
| CRM | Activity, marketing and support records |
| Entity | Relationship records and employee records |
| Item | Item records |
| Transaction body | The header of transactions |
| Transaction line | Transaction lines |
| Transaction item option | Options chosen per item on a transaction |
| Item number | Serial numbers and lot numbers |
| Other record | Custom records and other records |
| Other sublist | Extra columns on custom sublists |

The internal ID of a field (the ID a script uses) becomes visible in the UI once the user preference
that shows internal IDs is turned on.

Besides storing a plain value, a custom field can use these advanced options: encrypted values, values
computed by a summary search, dynamic defaults, dynamic hyperlinks, formula fields and SQL expressions.
The sub-topics go into defaults, formulas and summary searches.

A List/Record field can offer advanced PDF/HTML templates as its list. This exists for scripted custom
printing. The template list is not filtered per user, so every user sees every template.

## Field types

Available types: check box, currency, date, date/time, decimal number, document, email address,
free-form text, help, hyperlink, image, inline HTML, integer number, list/record, long text, multiple
select, password, percent, phone number, rich text, text area, time of day and URL fragment. The source
does not give the internal type IDs here; for SDF definitions see
[catalog:sdf_list_generic_customfield_fieldtype](../gen/catalogs/sdf_list_generic_customfield_fieldtype.md).

### Size and precision limits

Character limits are counted as if every letter took one byte. Text in a multibyte character set uses
more bytes per character, so fewer characters fit.

| Type | Maximum |
|---|---|
| Email address | 254 characters |
| Free-form text | 300 characters |
| Help | 999 characters |
| Hyperlink | 999 characters |
| Inline HTML | 4,000 bytes |
| Password | 15 characters |
| Phone number | 32 characters |
| Text area | 4,000 bytes |
| URL fragment | 200 characters |
| Long text | 1,000,000 characters through the UI; 100,000 characters when a script sets the value |
| Rich text | 1,000,000 characters |
| Currency | 15 digits before and 15 after the decimal point |
| Decimal number | 20 digits; extra decimal places are rounded down |
| Integer number | 20 digits |
| Percent | Whole numbers from 0 to 100 |

The long-text gap matters for scripts: the UI accepts a million characters, but a value written from
SuiteScript is capped at 100,000.

### Type-specific behaviour

- **Help**: the help text is displayed but not stored as data.
- **Date changed to date/time**: existing values get a time of midnight in the company's time zone.
- **Date/time fields added by script**: when a script adds a date/time field to a page with
  [`Form.addField()`](../modules/N_ui_serverWidget.md), use the time-zone-aware type
  (`serverWidget.FieldType.DATETIMETZ`, see [`serverWidget.FieldType`](../modules/N_ui_serverWidget.md)).
- **Entity fields**: an entity field is a List/Record field whose list is the entity type. It can
  point at contacts, customers, employees, groups, other names, partners and vendors.
- **Free-form text in arithmetic**: numbers kept in free-form text can turn into scientific notation
  when used in calculations; round them to avoid that.
- **Multiple select**: search results show at most 4,000 characters of the selected values.
- **Long text**: converting a field to long text is one-way; it cannot be undone.
- **Image**: accepts BMP, GIF, JPG, JPEG, PJPG, PJPEG, PNG, TIFF and TIF. The image is rendered no wider
  than 250 pixels.
- **Inline HTML**: the field has no label, and its content cannot be scripted through the DOM.
- **Password**: the value is stored encrypted. Code that checks a password must compare encrypted
  values, not plain text.
- **Document**: working with the field requires access to the File Cabinet.
- **Hyperlink**: the source says the link text is limited to 10 characters.

> [!SOURCE-ERROR] The 10-character cap on hyperlink link text looks doubtful, especially next to the
> 999-character limit for the hyperlink value itself. Test the real limit in your account before you
> rely on it.
<!-- source-error: topic:custom_fields | - | link_text_limit_10_characters_doubtful -->

- **URLs stored in fields**: use the account-specific domain. Data center URLs are rewritten when pages
  are served, but the configured value itself is not changed.

## Encrypted storage

Since 2014.1, a field definition has an option to store values in encrypted format. The option is off
by default. When it is on, values are encrypted in the database but users still see them normally in
the UI.

- Types that can be encrypted: email address, free-form text, long text, phone number, rich text and
  text area.
- The encryption setting cannot be changed once the field has been saved.
- Encrypted format is not supported on transaction item option fields, nor on fields of Suitelets,
  workflows or workflow states.
- Viewed from any account other than the one where it was stored, a value shows in its encrypted form.
- Encrypted values are not available to reporting, SuiteAnalytics Connect, sourcing or filtering.
- Encrypted rich-text and long-text values do not appear in search results.
- The SuiteScript 1.0 function [`nlapiLookupField`](../ss1/fields-sublists.md) does not support
  encrypted values, and other APIs that work through search may fail on them. The source does not name
  a replacement approach.
- System notes mask encrypted values.

## Example

A Suitelet that offers a delivery appointment form. The appointment field is added as a time-zone-aware
date/time field, as recommended for date/time fields created by script.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget'], (serverWidget) => {
  const onRequest = (context) => {
    const form = serverWidget.createForm({ title: 'Book a Delivery Slot' });
    form.addField({
      id: 'custpage_slot_start',
      label: 'Preferred arrival',
      type: serverWidget.FieldType.DATETIMETZ
    });
    form.addField({
      id: 'custpage_dock_note',
      label: 'Dock instructions',
      type: serverWidget.FieldType.TEXTAREA
    });
    form.addSubmitButton({ label: 'Request slot' });
    context.response.writePage({ pageObject: form });
  };

  return { onRequest };
});
```
