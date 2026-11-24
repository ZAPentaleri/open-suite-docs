# Search formulas
<!-- id: topic:search_formulas -->

Sub-topics:

- [Search formula techniques](search_formulas.techniques.md)

See also: [Saved searches](saved_searches.md), [N/search](../modules/N_search.md),
[`Column.formula`](../modules/N_search.md#columnformula), [`Filter.formula`](../modules/N_search.md#filterformula).

A search formula is a SQL expression worked out while a search runs. Formulas can appear in three
places: custom fields defined by a formula, search criteria (filters) and result columns.

## How formulas are evaluated

The expression is handed to NetSuite's Oracle database when the search runs, so the value is computed
at run time rather than stored. Not every Oracle function is supported; the allowed ones are listed in
the [search formula functions catalog](../gen/catalogs/search_formula_functions.md).

A formula is put together from three kinds of element:

- field IDs written in braces, such as `{trandate}` or `{amount}`,
- SQL functions from the catalog, and
- arithmetic operators.

Rules that follow from this:

- Refer to fields by their internal IDs, not by the translated labels the UI shows.
- Join strings with `||`.
- A formula cannot mix aggregate functions with non-aggregate expressions. In an aggregate formula,
  every field must either sit inside an aggregate function or be one of the grouped fields.
- Encrypted fields cannot be used in a formula; including one makes the expression invalid.
- A formula is limited to 1,000 characters, both in criteria and in result columns. A criteria
  formula over that length raises an error when the search runs.

## Formula types

| Where | Formula field IDs |
|---|---|
| Criteria (filters) | `formuladate`, `formulanumeric`, `formulatext` |
| Result columns | `formulacurrency`, `formuladate`, `formuladatetime`, `formulanumeric`, `formulapercent`, `formulatext`, `formulahtml` |

In a script, pass the formula field ID as `name` and the expression as `formula` to
[`search.createColumn`](../modules/N_search.md#searchcreatecolumnoptions) or
[`search.createFilter`](../modules/N_search.md#searchcreatefilteroptions).

### Text versus HTML results

A text formula's result is displayed as plain text, markup and all. To have HTML rendered, use a
`formulahtml` column instead. Script tags are never executed, neither in text formulas nor in HTML
ones.

HTML formula columns depend on the `HTML Formulas in Search` feature and on the "Create HTML Formulas
in Search" permission (`ADMI_HTMLFORMULA`; see the [permissions catalog](../gen/catalogs/permissions.md)).
If someone without that permission edits a search, its HTML formula columns are removed.

### Workarounds that use formulas

- Integer number fields are shown with at most 13 digits. For longer values, return the field through a
  `formulatext` column.
- A filter on quantity silently leaves out negative quantities. To keep them, filter with
  `formulanumeric` on the quantity field instead.

## Formula fields on records

A custom field defined by a formula behaves differently depending on whether its value is stored:

- A **stored** formula field keeps the value computed when the record was created and does not follow
  later changes.
- An **unstored** formula field is not available in search results.

## Variables for the running user

A formula can refer to the context of the user running the search through variables written in
braces. Each variable has synonyms that behave the same way.

| Variable | Synonyms | Resolves to |
|---|---|---|
| `{today}` | `{now}` | Today's date |
| `{me}` | `{user}`, `{user.id}` | The running user |
| `{userrole}` | `{userrole.id}` | The user's role |
| `{user.department}` | `{userdepartment.id}` | The user's department |
| `{user.location}` | `{userlocation.id}` | The user's location |
| `{user.subsidiary}` | `{usersubsidiary.id}` | The user's subsidiary |
| `{user.class}` | `{userclass.id}` | The user's class |
| `{usercurrency}` | `{usercurrency.id}` | The user's currency |

Another part of the source lists the same set as tags in upper case, and there the currency entry is
`USER.CURRENCY` with the synonym `USERCURRENCY`.

> [!SOURCE-ERROR] The two source lists disagree on the currency variable: one gives `usercurrency`
> with the synonym `usercurrency.id`, the other gives `user.currency` with the synonym `usercurrency`.
> The spec does not say which is right; test the exact spelling in a formula before relying on it.
<!-- source-error: topic:search_formulas | - | currency_synonym_differs_from_variables_table -->

## Example

A scheduled script listing open invoices on which the running user is the sales rep, with three
formula columns: a label built by concatenation, the share still unpaid (guarding against division by
zero), and the days past due. The `CASE` filter compares `{salesrep.id}` with `{me}`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const execute = () => {
    const label = search.createColumn({
      name: 'formulatext',
      formula: "{tranid} || ' / ' || {entity}",
      label: 'Invoice and customer'
    });
    const unpaidShare = search.createColumn({
      name: 'formulapercent',
      formula: '{amountremaining} / NULLIF({amount}, 0)',
      label: 'Unpaid share'
    });
    const daysLate = search.createColumn({
      name: 'formulanumeric',
      formula: 'ROUND({today} - {duedate})',
      label: 'Days past due'
    });

    const mine = search.create({
      type: search.Type.INVOICE,
      filters: [
        search.createFilter({ name: 'mainline', operator: search.Operator.IS, values: 'T' }),
        search.createFilter({ name: 'amountremaining', operator: search.Operator.GREATERTHAN, values: 0 }),
        search.createFilter({
          name: 'formulanumeric',
          operator: search.Operator.EQUALTO,
          values: 1,
          formula: 'CASE WHEN {salesrep.id} = {me} THEN 1 ELSE 0 END'
        })
      ],
      columns: [label, unpaidShare, daysLate]
    });

    mine.run().each((row) => {
      log.debug({
        title: row.getValue(label),
        details: `${row.getValue(unpaidShare)} unpaid, ${row.getValue(daysLate)} days late`
      });
      return true;
    });
  };

  return { execute };
});
```
