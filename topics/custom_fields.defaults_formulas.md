# Custom field defaults and formulas
<!-- id: topic:custom_fields.defaults_formulas -->

Part of: [Custom fields](custom_fields.md)

See also: [Search formulas](search_formulas.md), [Custom field sourcing](custom_fields.sourcing.md).

A custom field can get its value from an expression instead of user input. The expression combines
NetSuite tags (field references in braces) with SQL expressions. Two flags on the field decide how
that expression behaves: the **Formula** box (treat the expression as SQL) and **Store Value** (keep the
result in the database).

## Tags: referring to other fields

- Write a tag as the field's ID in braces, for example `{custbody_route_code}`. Any field that has a
  script ID can be used as a tag.
- For a List/Record field, `{field.id}` gives the internal ID of the referenced record.
- To reach a field on a related record, chain the IDs: `{field_on_record.field_on_joined_record}`. Only
  relationships that exist as search joins can be followed this way.
- In a formula, the tag for the line number is `{linenumber}`. Use it on an integer field with Store
  Value cleared. While the record is in edit mode the line number shows as 1; when the record is viewed
  or printed the numbers are contiguous.
- The tag for the memo field behaves specially: it returns the body memo, and if that is empty, the
  memo of the first line that has one.
- The `memobody` placeholder only works for related records and cannot be read from SuiteScript.
- Give custom fields meaningful IDs; they are what you type into these expressions.

### User and date tags in formulas

These tags, each written in braces, are available in formulas: `TODAY`, `NOW`, `ME`, `USER`,
`USER.ID`, `USERROLE`, `USERROLE.ID`, `USER.DEPARTMENT`, `USERDEPARTMENT.ID`, `USER.LOCATION`,
`USERLOCATION.ID`, `USER.SUBSIDIARY`, `USERSUBSIDIARY.ID`, `USER.CLASS` and `USERCLASS.ID`.

### Special tags when the Formula box is cleared

When the Formula box is not checked, a different set of special tags is substituted as plain text:
`useremail`, `now`, `today`, `nlversion`, `nlsessionid`, `nluser` and `nlrole`.

## Dynamic defaults (Formula box cleared)

Free-form text, text area, rich text and hyperlink fields can take a dynamic default: text with tags
in it, substituted when the page loads and when it is saved.

- A hyperlink default can carry tags in its query string, which builds a link per record. Keep the
  Formula box cleared for such URLs; if it is checked, the URL is evaluated as SQL and fails with an
  invalid-expression error (in SQL you would have to join the parts with `||`).
- Every field used as a tag inside a link should be mandatory. If one is empty, the resulting URL is
  invalid.
- With Store Value checked, the default is saved once and never re-evaluated. Clear Store Value if you
  want the tags substituted again every time the record loads.
- A stored default is set only when a new record's form is loaded, so it does not update records that
  already exist.

## Formulas (Formula box checked)

- The system evaluates a formula by first substituting the tags and then running the expression as a
  `SELECT ... FROM DUAL` statement. Check the Formula box only when the expression is SQL.
- The Formula box may get checked automatically; verify its state before saving.
- To make a formula recalculate, check the Formula box and clear Store Value. Even then it recalculates
  only when the record is loaded again.
- A field referenced by a formula field cannot be edited inline.

### Store Value and recalculation

| Store Value | Effect |
|---|---|
| Checked | The first computed value is kept and never updated |
| Cleared | The value is computed again each time the record is loaded and saved |

Clear Store Value for any formula that must stay current.

### Techniques

- Format dates with `TO_CHAR` and the `FM` format modifier to drop padding.
- Wrap amounts in `NVL` so empty currency values count as zero.
- Use `NULLIF` on the divisor to avoid division-by-zero errors.

### Errors

| Error | Cause |
|---|---|
| Field not found | The formula references a field that does not exist or that the user cannot access |
| Invalid formula | A syntax error, or values of mismatched types |

## Formula fields in searches

A formula field whose value is not stored does not appear in search results.

> [!SOURCE-ERROR] The source says unstored formula fields are missing from search results, yet elsewhere
> shows such fields with calculated values in results. Test in your account whether the field appears
> before building a search that depends on it.
<!-- source-error: topic:custom_fields.defaults_formulas | - | unstored_formula_absent_from_searches_yet_shown_calculated -->

## Example

A sales order body field showing the estimated margin as a percentage. Make it a percent field, check
Formula, clear Store Value, and enter:

```
(NVL({total}, 0) - NVL({totalcostestimate}, 0)) / NULLIF({total}, 0) * 100
```

`NVL` treats a missing amount as zero, and `NULLIF` makes the result empty instead of failing when the
order total is zero.
