# Custom lists
<!-- id: topic:custom_lists -->

See also: [Custom fields](custom_fields.md), [Custom records](custom_records.md),
[CSV import](csv_import.md), [SuiteCloud Development Framework](sdf.md).

A custom list holds a small, fixed set of choices that you then offer to users through custom fields.
If the set of values is large or keeps growing, a list is the wrong tool; the size guidance below
explains why.

## Size

- The source sets no hard cap: an account may have any number of lists, and a list any number of
  values. It does recommend keeping a single list at 1,000 values or fewer.
- When a list grows beyond 1,000 values, you no longer edit the values on the list itself; you edit
  them through a separate manage-values page.
- A very large list shows its values in alphabetical order, a page at a time, and only for viewing.
- You can load a large list through CSV import, but not one with more than 25,000 values.

## Order and who can change a list

- By default values keep the order in which they were entered; you can switch the list to
  alphabetical order instead.
- A list can be changed by its owner and by users who have Edit or Full permission for it.

## Other uses

- A list can act as a matrix option list. This requires the `Accounting Matrix Items` feature, and
  turning the list into a matrix option list adds an abbreviation column to it.
- Lists can be copied to another account and managed through [SDF](sdf.md).
- The abbreviation column and the translation columns cannot be edited inline.

## Translations

Translating a list's name and its values requires the `Multi-Language` feature. Translations have
length limits:

| What is translated | Maximum length |
|---|---|
| List name | 30 characters |
| List value | 60 characters |

Only translate into languages that the system supports.
