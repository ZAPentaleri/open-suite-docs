# Query Relative Dates
<!-- id: topic:query_definition.relative_dates -->

Part of: [Query definitions and components](query_definition.md)

See also: [N/query](../modules/N_query.md).

Query conditions can compare date fields against points or periods that are defined relative to
today, so the same query keeps working as time passes.

## Two kinds of relative value

- A [`query.RelativeDate`](../modules/N_query.md) stands for a single moment measured from today (for
  example "five days ago"). Build one with `query.createRelativeDate()` and a date ID.
- A value from [`query.RelativeDateRange`](../modules/N_query.md) stands for a whole period measured
  from today (for example "last month").

## Operators that accept them

Use relative values with the date operators of [`query.Operator`](../modules/N_query.md):

| Operator | Negated form |
|---|---|
| `AFTER` | `AFTER_NOT` |
| `BEFORE` | `BEFORE_NOT` |
| `ON` | `ON_NOT` |
| `ON_OR_AFTER` | `ON_OR_AFTER_NOT` |
| `ON_OR_BEFORE` | `ON_OR_BEFORE_NOT` |
| `WITHIN` | `WITHIN_NOT` |

## How WITHIN reads its values

- Given a single relative date, `WITHIN` matches the span from that date up to today.
- Given a range value, `WITHIN` uses the range's own start and end.

So for a range value, these three ways of writing the condition value mean the same thing:

- the range value itself;
- the range's `interval` property;
- a two-element array of the range's `start` and `end`.

You can also mix range values and relative dates in one values list.

## Example

Find sales orders dated within last month, and separately those dated in the window that runs from two
weeks ago up to today. Field IDs are illustrative; confirm them in the
[Records Catalog](records_catalog.md).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const countMatches = (condition) => {
    const q = query.create({ type: query.Type.TRANSACTION });
    const isOrder = q.createCondition({ fieldId: 'type', operator: query.Operator.ANY_OF, values: ['SalesOrd'] });
    q.condition = q.and(isOrder, condition(q));
    q.columns = [q.createColumn({ fieldId: 'id' })];
    return q.run().results.length;
  };

  const execute = () => {
    const lastMonth = countMatches((q) => q.createCondition({
      fieldId: 'trandate',
      operator: query.Operator.WITHIN,
      values: [query.RelativeDateRange.LAST_MONTH]
    }));

    const twoWeeks = countMatches((q) => q.createCondition({
      fieldId: 'trandate',
      operator: query.Operator.WITHIN,
      values: [query.createRelativeDate({ dateId: query.DateId.WEEKS_AGO, value: 2 })]
    }));

    log.audit({ title: 'Sales orders', details: `Last month: ${lastMonth}; last two weeks: ${twoWeeks}` });
  };

  return { execute };
});
```
