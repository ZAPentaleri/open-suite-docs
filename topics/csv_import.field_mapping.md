# CSV import data handling and field mapping
<!-- id: topic:csv_import.field_mapping -->

Part of: [CSV import](csv_import.md)

This page covers the choices that decide what an import does to your data: the data handling mode,
the file mapping step of multi-file imports, and the field mapping step. Working through these
steps in the Import Assistant requires the `Import CSV File` permission (`ADMI_IMPORTCSVFILE`).

## Before you map

- You do not need a template file. Any CSV with a header row will do.
- If you name your CSV columns after the NetSuite fields they hold, the assistant can map many of
  them for you automatically.
- To find out which field a column should go to, use the field list on the field mapping page or
  the field help on the record's form.
- **OneWorld:** set up your subsidiaries before you import. A subsidiary is required when importing
  the chart of accounts, contacts, customers, employees, jobs, leads, partners, prospects and
  vendors, and an import of those types fails if no subsidiary is given.

## Data handling modes

| Mode | Use it when |
|---|---|
| Add | Every row is a new record. |
| Update | Every row refers to a record that already exists. |
| Add or Update | The file mixes new and existing records. |

When updating:

- A blank value in the CSV leaves the field unchanged, unless *Overwrite missing fields* is on (see
  [advanced options](csv_import.advanced_options.md)).
- Fields that depend on other fields are sourced just as they would be in the UI. On updates,
  include all the related values in the file so that sourcing does not leave you with values you
  did not intend.
- Blank rows do not delete sublist lines.

## File mapping (multi-file imports only)

The file mapping step appears only when you uploaded a primary file plus linked files. For each
linked file you tie one of its columns (its key column) to the key column of the primary file; that
link is how each sublist or subrecord row finds its parent record.

## Field mapping

The assistant marks fields that must be filled with "req".

- **Required fields.** Every required field must be mapped to a column or given a default value, and
  you cannot leave the field mapping step while any required field is still unmapped. If the CSV
  has no data for a required field, you have three ways out: map a blank column to it, set it to
  null, or give it a default value. Required fields of a sublist count only if you are importing
  that sublist.
- **Which fields show up.** The editable fields of the role's preferred form. Choosing a custom form
  in the advanced options widens that set.
- **What you can do with a field.** Map a CSV column to it, give it a default value, set it to null,
  or choose the reference type used to resolve its value.
- **One column, several fields.** A single CSV column can be mapped to more than one field.
- **Repeated sublists in one file.** Up to 99 instances of a sublist can be mapped from a single
  file. This does not apply when the sublist comes from linked files.
- **Item options** on transaction imports are found under the Items sublist folder of the field
  list.
- **Unmapped sublists import nothing,** even if you gave some of their fields default values.
- **Marketing automation:** with that feature on, Add imports must supply the unsubscribe field.

## Reference types

When a field points at another record, you tell the assistant how the CSV value identifies that
record:

| Reference type | Notes |
|---|---|
| External ID | Recommended for Add and Add or Update imports. Not offered for custom lists and custom records. |
| Internal ID | Recommended for Update imports. Internal IDs are not handed out in date order, so use the date-created field, not the ID, when you need chronology. |
| Name | Not case sensitive. For a child in a hierarchy, the value must include the parent's name. |
| Script ID | For custom list values and custom record instances. A custom record's script ID can be set only by SuiteScript, at the moment the record is created. |

See [custom lists](custom_lists.md) and [custom records](custom_records.md).

## Default values

- A default value is applied to every record the import touches.
- A default only works for values that already exist in the account; set them up first.
- The dropdown you pick a default from is not filtered by dependent fields. In OneWorld accounts,
  for example, the tax code list can offer codes that do not belong to the record's subsidiary.
- Unlike the UI, an import of customers, leads or prospects does not make the importing user the
  primary sales rep.

## Setting a field to null

Choosing null for a field clears whatever value the record already holds. Use it to stop a default
from the UI being applied, or to blank out fields on Update imports where the CSV values are empty.
