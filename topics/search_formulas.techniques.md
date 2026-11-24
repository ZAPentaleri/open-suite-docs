# Search formula techniques
<!-- id: topic:search_formulas.techniques -->

Part of: [Search formulas](search_formulas.md)

See also: [N/search](../modules/N_search.md), [Saved search definition options](saved_searches.definition.md).

This page collects practical patterns for formula criteria and formula result columns in saved
searches and in scripted searches built with [N/search](../modules/N_search.md). The functions you may
call are listed in the [search formula functions catalog](../gen/catalogs/search_formula_functions.md);
the general rules for formulas (braces, length limit, formula types) are on the parent page.

## Referring to fields

| What you want | How to write it | Notes |
|---|---|---|
| A field of the record being searched | `{field.id}`, i.e. the field ID in braces | This is how you reach the fields listed for the record type |
| A field of a joined record | `{join.field}` | The join name, a dot, then the field on the joined record; for example the credit limit of the vendor joined to a transaction is written `{vendor.creditlimit}` |
| A field of the record's system notes | `{systemnotes.field}` | System notes are the change history kept for a record |

The source names only two system-notes fields in its material, `{systemnotes.name}` (who made the
change) and `{systemnotes.date}` (when). It does not list the other system-notes fields you can
reference, so check the join's available fields in your account before relying on any other name.

## Dividing safely

When a formula divides one value by another, wrap the denominator in `NULLIF(<denominator>, 0)`. A
zero denominator then becomes empty, and (as in Oracle SQL generally) the division yields an empty
result for that row instead of failing the whole expression.

## Line sequence numbers

Do not assume line sequence numbers come back in order. The source warns that they may be
unordered, so sort explicitly on a column you control when line order matters.

## Ranking with DENSE_RANK

- `DENSE_RANK` numbers records consecutively by the value of a field you choose: rows with equal
  values share a rank, and the next distinct value gets the next number, without gaps.
- Combined with `KEEP` (`KEEP (DENSE_RANK FIRST ...)` or `KEEP (DENSE_RANK LAST ...)`), it keeps only
  the record ranked first or last, so an aggregate returns the value from that one record. The
  general shape is `<aggregate>(<value>) KEEP (DENSE_RANK LAST ORDER BY <ordering field>)`, which
  returns the value from the latest row by the ordering field.

### Who last changed a record

The source describes one concrete use of the `KEEP ... DENSE_RANK LAST` pattern: showing the person
who most recently updated each record. Its ingredients are:

- a search whose type the source gives only as "document";
- a result column of the formula (text) type, with the summary type set to minimum;
- the aggregate `MIN` applied to `{systemnotes.name}`, kept to the last rank when ordered by
  `{systemnotes.date}`.

Because the column carries a summary type, the other result columns need summary types too (see the
aggregate rules on the [parent page](search_formulas.md)).

## Totals per fiscal year

To bucket amounts by fiscal year in one column, combine `DECODE` with `ADD_MONTHS` and `TO_CHAR`. The
source names only the combination; the idea is to shift each date by the number of months between the
start of your fiscal year and the start of the calendar year (`ADD_MONTHS`), take the year of the
shifted date as text (`TO_CHAR`), and let `DECODE` return the amount when that year is the fiscal year
you want and zero otherwise. Summing such a column gives the fiscal-year total. Work out the month
offset for your own fiscal calendar and test it against a known period.

## Example: credit utilization review

A scheduled script that lists customers who have used at least 90 percent of their credit limit. The
utilization is computed in a formula column, open balance plus unbilled orders divided by the credit
limit, with `NULLIF` guarding the division. Field IDs come from the
[customer record page](../gen/records/customer.md) (search filters and columns).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const UTILIZATION = 'ROUND(({balance} + {unbilledorders}) / NULLIF({creditlimit}, 0), 2)';
  const ALERT_LEVEL = 0.9;

  const execute = () => {
    const review = search.create({
      type: search.Type.CUSTOMER,
      filters: [['creditlimit', search.Operator.GREATERTHAN, 0]],
      columns: [
        'entityid',
        'creditlimit',
        search.createColumn({ name: 'formulanumeric', formula: UTILIZATION })
      ]
    });

    let flagged = 0;
    review.run().each((result) => {
      const used = Number(result.getValue({ name: 'formulanumeric' }));
      if (used >= ALERT_LEVEL) {
        flagged += 1;
        log.audit({
          title: `Credit nearly used: ${result.getValue({ name: 'entityid' })}`,
          details: `Limit ${result.getValue({ name: 'creditlimit' })}, utilization ${used}`
        });
      }
      return true;
    });

    log.audit({ title: 'Credit review finished', details: `${flagged} customer(s) flagged` });
  };

  return { execute };
});
```
