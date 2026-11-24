# Custom segments
<!-- id: topic:custom_segments -->

Sub-topics:

- [Where custom segments apply](custom_segments.application.md)
- [Custom segments in workflows](custom_segments.workflows.md)

See also: [Custom transactions](custom_transactions.md), [Custom records](custom_records.md),
[Customization records](records_guide.customization.md), [CSV import](csv_import.md),
[N/record](../modules/N_record.md), [N/search](../modules/N_search.md).

Custom segments are classifications that an account defines itself and applies to records and
transactions. They exist only when the `Custom Segments` feature is enabled.

## IDs

- Segment IDs begin with `cseg`. Choose the rest of the ID so it says what the segment is for.
- A segment has one script ID, shared by every record type it is applied to, so a script uses the
  same field ID wherever the segment appears.
- Search IDs are different: they reveal what kind of field you are looking at (body field, line
  field or filter field).
- A field that filters one segment by another classification has the shape
  `{cseg_x.cseg_x_filterby_subsidiary}`: the segment ID, a dot, then the segment ID again with
  `_filterby_` and the name of the classification doing the filtering.

## Scripting

| What | Scriptable? |
|---|---|
| Reading and setting a segment's value on a record | Yes |
| Creating new segment values | Yes |
| Segment definitions | No; segments also do not appear in a script deployment's list of record types it applies to |

The values of a segment live in a custom record type named after the segment, and that record type
is fully scriptable. The source does not state the ID a script passes to
[N/record](../modules/N_record.md) for that record type, nor which call creates a value; check the
record type in your account.

## Searches

- Segments can be used as search filters and as search columns; in those lists their labels end
  with "(Custom)".
- Transaction searches list each segment twice: once as a body field and once as a line (column)
  field.
- Using a segment as a filter or a column needs Edit at the segment's search/reporting access
  level. Seeing it in result columns needs only View at that level.
- To search the segment definitions themselves, use the custom segment search type.

## CSV import of segment values

- The CSV import assistant lists each segment as a record type under the classification import type.
- Importable value fields: name, parent, inactive and translations.
- The importing role needs the custom segments permission at View, plus the segment's value access
  level at Create to add values, or at Edit to update values or set a parent.
- Out of the box, only administrators can import segment values.

## Supported transactions

Entries are at body level unless the source names another part (lines, expense columns or time
columns).

| Transaction | Where |
|---|---|
| Assembly build | Body |
| Bad debt | Lines |
| Bill | Body; expense columns |
| Bill credit | Body; expense columns |
| Bin putaway worksheet | Body |
| Bin transfer | Body |
| Blanket purchase order | Body; expense columns |
| Budget | Body |
| Cash refund | Body |
| Cash sale | Body |
| Check | Body; expense columns |
| Commission | Body |
| Credit card | Body; expense columns |
| Credit card refund | Body |
| Credit memo | Body |
| Currency revaluation | Body |
| Custom transactions | Expense columns |
| Customer payment | Body |
| Deposit application | Body |
| Deprecated custom transactions | Body |
| Estimate | Body |
| Expense report | Lines |
| Expense columns (no transaction named) [sic] | As listed |
| Finance charge | Body |
| Inventory [sic] | Body |
| Inventory adjustment | Lines |
| Inventory count | Body |
| Inventory distribution | Body |
| Inventory part [sic] | Lines |
| Inventory transfer | Lines |
| Invoice | Body |
| Item fulfillment | Body; lines |
| Item receipt | Expense columns; lines |
| Journal entry | Lines |
| Opportunity | Lines |
| Other charge item [sic] | Lines |
| Partner commission | Body |
| Payable tegata | Body |
| Paycheck | From time entries |
| Paycheck earning | Body; lines from time entries |
| Paycheck journal | Body |
| Payment end journal | Body |
| Payroll adjustment | Body |
| Payroll liability check | Body |
| Period end journal | Body; lines |
| Purchase contract | Body |
| Purchase order | Body; expense columns |
| Receivable tegata | Body |
| Request for quote | Body |
| Requisition | Body; expense columns |
| Return authorization | Body |
| Revenue arrangement | Body |
| Revenue contract | Body |
| Revenue commitment | Body |
| Revenue commitment reversal | Body |
| Sales order | Body |
| Sales tax payment | Body |
| Statement charge | Body |
| Store pickup fulfillment | Body |
| Tax liability check | Body |
| Time tracking | Time columns |
| Transfer order | Body; lines |
| Vendor payment | Body |
| Vendor request for quote | Body |
| Vendor return authorization | Body; expense columns |
| Work order | Body; lines |

Line item types that support segments: apply-to-kit, assembly, item fulfillment [sic], item group,
kit item, non-inventory part, nonmodifiable type, service, and store-with-item-groups.

> [!SOURCE-ERROR] The source's transaction list mixes in entries that are item types or column kinds
> rather than transactions (the rows marked [sic] above), and its item type list includes item
> fulfillment, which is a transaction. The entries are shown as given; check them in your account
> before you rely on them.
<!-- source-error: topic:custom_segments | - | item_types_and_column_kinds_in_transaction_list -->

## Example

A user event script that copies a sales order's body-level segment value onto every item line that
has none. Because one script ID serves body and lines alike, the same field ID works in both places.
The segment ID `cseg_sales_channel` is an invented, account-specific ID.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/log'], (log) => {
  const SEGMENT_ID = 'cseg_sales_channel';

  const beforeSubmit = (context) => {
    const order = context.newRecord;
    const channel = order.getValue({ fieldId: SEGMENT_ID });
    if (!channel) {
      return;
    }
    const lines = order.getLineCount({ sublistId: 'item' });
    let filled = 0;
    for (let line = 0; line < lines; line += 1) {
      const current = order.getSublistValue({ sublistId: 'item', fieldId: SEGMENT_ID, line });
      if (!current) {
        order.setSublistValue({ sublistId: 'item', fieldId: SEGMENT_ID, line, value: channel });
        filled += 1;
      }
    }
    log.debug({ title: 'Segment copied to lines', details: `${filled} of ${lines}` });
  };

  return { beforeSubmit };
});
```
