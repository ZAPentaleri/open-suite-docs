# Query Formulas
<!-- id: topic:query_definition.formulas -->

Part of: [Query definitions and components](query_definition.md)

See also: [N/query](../modules/N_query.md), [Records Catalog](records_catalog.md),
[Search formulas](search_formulas.md).

In a query definition built with [N/query](../modules/N_query.md), a formula can take the place of a
plain field ID in two places: in **columns** (`createColumn()`) and in **conditions**
(`createCondition()`). Inside the formula string, each field reference is written as the field ID in
curly braces; the spec draws this from its examples rather than stating it as a rule.

## Always give a return type

Whenever you use a formula, also pass the type parameter, set to a value of
[`query.ReturnType`](../modules/N_query.md#queryreturntype). The reason is that NetSuite cannot
always work out on its own what kind of value an expression produces. The type you pass also decides
how the returned value is formatted.

## Referring to joined fields inside a formula

In a query that uses joins, a formula that reads a field on a related record must spell out the full
join trail, starting at the query's base record type. There are three notations, one per kind of
join:

| Join kind | Pattern | Meaning |
|---|---|---|
| Automatic join | `base.joinfield.field` | Follow `joinfield` from the base record, then read `field` |
| Polymorphic join | `base.field^target.field` | The caret `^` follows a field that can point at more than one record type and names the target type, the way `joinTo()` does |
| Inverse join | `base.field<source.field` | The less-than sign `<` walks the relationship backwards from the source record, the way `joinFrom()` does |

- If the trail uses only automatic joins, no operator is needed; dots are enough.
- You may add whitespace and parentheses inside a join trail to make it easier to read.
- The `^` and `<` operators also work in formulas typed into the NetSuite UI.

## What formulas can express

The spec illustrates these shapes (described here in words; write and test your own expressions):

- adding up three custom stock-level fields into one numeric value;
- reading the phone number of a customer's sales rep through an automatic join;
- following a transaction's created-by field with `^` to the employee record and returning that
  employee's email;
- starting from an employee and walking back with `<` to a field on the transactions that reference
  it.

## Example

Return each customer's name plus a formula column that reads the sales rep's phone through an
automatic join, with the trail written from the base record type and the return type given
explicitly. Field IDs for N/query come from the [Records Catalog](records_catalog.md), not from the
N/search record pages of this reference; confirm the expression in your account.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/query', 'N/log'], (query, log) => {
  const execute = () => {
    const q = query.create({ type: query.Type.CUSTOMER });
    q.columns = [
      q.createColumn({ fieldId: 'companyname' }),
      q.createColumn({
        formula: '{customer.salesrep.phone}',
        type: query.ReturnType.STRING
      })
    ];
    q.run().results.forEach((row) => {
      log.debug({ title: row.values[0], details: `Rep phone: ${row.values[1]}` });
    });
  };

  return { execute };
});
```
