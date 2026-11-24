# N/manufacturing/productionCharges
<!-- id: N/manufacturing/productionCharges -->

`N/manufacturing/productionCharges` rewrites the unit costs of production charges on manufacturing
transactions after the fact, for example to bring charges up to date with current purchase prices or
to apply a negotiated cost. You point it at a transaction (and, for two of the functions, specific
lines) and it updates the costs in place.

Prerequisites and behaviour that apply to the whole module:

- Server scripts only, SuiteScript 2.1 only.
- The running role needs Edit permission on build-assembly and work-in-progress (WIP) transactions.
- The transaction's posting period must be open.
- The account preference that allows bulk cost updates for production charges must be turned on.
- Changing costs through this module does not trigger other scripts on the transaction.
- The module overview names assembly builds, work order closes and work order completions as the
  transactions it applies to, while the individual functions name assembly builds, work order
  completions and work order issues (see the callout).

> [!SOURCE-ERROR] The source's introduction lists work order close among the supported transactions,
> where the per-function descriptions list work order issue instead. The spec does not say which list
> is right; test against the transaction type you need before relying on it.
<!-- source-error: N/manufacturing/productionCharges | - | intro_lists_work_order_close_prerequisites_list_issue -->

- **Load it as:** `define(['N/manufacturing/productionCharges'], (productionCharges) => { ... })`
- **Since:** 2026.1

## Members

| Member | Kind | Summary |
|---|---|---|
| [`productionCharges.updateAllChargesToItemPurchasePrice(options)`](#productionchargesupdateallchargestoitempurchasepriceoptions) | function | Every charge line to the current purchase price |
| [`productionCharges.updateChargesToCustomUnitCost(options)`](#productionchargesupdatechargestocustomunitcostoptions) | function | Chosen lines to a unit cost you supply |
| [`productionCharges.updateChargesToItemPurchasePrice(options)`](#productionchargesupdatechargestoitempurchasepriceoptions) | function | Chosen lines to the current purchase price |

## Functions

### productionCharges.updateAllChargesToItemPurchasePrice(options)
<!-- id: N/manufacturing/productionCharges.updateAllChargesToItemPurchasePrice -->

Sets every routing line and non-inventory charge line on the transaction to the item's current
purchase price, in one call. Applies to assembly builds, work order completions and work order
issues.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.transactionId` | number | Yes | |

**Returns:** `void`

**Errors:**
- `INVALID_NUMBER_MUST_BE_GREATER_THAN_1`: `options.transactionId` is not a positive integer.
- `FEATURE_1_MUST_BE_ENABLED_TO_USE_2_API`: the assembly items feature is disabled.
- `PREFERENCE_1_REQUIRED_FOR_THIS_OPERATION`: the bulk cost update preference for production charges
  is off.
- `ACCESS_DENIED`: the role cannot edit manufacturing transactions.
- `TRANSACTION_1_IS_INVALID_OR_HAS_NO_EDITABLE_TRANSACTION_LINES`: the transaction is invalid or has
  no lines that can be edited.

> [!SOURCE-ERROR] The source's description of the invalid-number error mentions transaction line IDs,
> but this function takes only a transaction ID. Read the error as applying to `transactionId`.
<!-- source-error: N/manufacturing/productionCharges.updateAllChargesToItemPurchasePrice | - | error_mentions_transactionlineids_not_a_parameter -->

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2026.1

- The posting period must be open; an invalid transaction or lines result in errors rather than a
  partial update being reported.

### productionCharges.updateChargesToCustomUnitCost(options)
<!-- id: N/manufacturing/productionCharges.updateChargesToCustomUnitCost -->

Sets the cost of the listed lines to a unit cost you provide. Lines it can change: credit lines;
non-inventory, service or other-charge items; items bought for purchase or resale; and lines whose
cost category is anything other than outsourcing or landed-cost service. Applies to assembly
builds, work order completions and work order issues. Needs the "allow bulk cost updates for
production charges" preference.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.isUnitCostPerBaseUnit` | boolean | No | `true` |
| `options.transactionId` | number | Yes | |
| `options.transactionLineIds` | number[] | Yes | |
| `options.newUnitCost` | number | Yes | |

- `options.isUnitCostPerBaseUnit`: `true` treats `newUnitCost` as the cost per base unit of
  measure; `false` applies it as a fixed cost and ignores unit-of-measure conversion rates. Only
  relevant with the Multiple Units of Measure feature.
- `options.transactionLineIds`: the lines to change, as positive integers.

**Returns:** `void`

**Errors:**
- `INVALID_NUMBER_MUST_BE_GREATER_THAN_1`: `options.transactionId` or an entry of
  `options.transactionLineIds` is not a positive integer.
- `FEATURE_1_MUST_BE_ENABLED_TO_USE_2_API`: the assembly items feature is disabled.
- `PREFERENCE_1_REQUIRED_FOR_THIS_OPERATION`: the bulk cost update preference is off.
- `ACCESS_DENIED`: the role cannot edit manufacturing transactions.
- `TRANSACTION_1_IS_INVALID_OR_HAS_NO_EDITABLE_TRANSACTION_LINES`: the transaction is invalid or has
  no editable lines.
- `PROVIDED_TRANSACTION_LINES_ARE_NOT_EDITABLE_OR_INVALID_1`: one or more of the given lines cannot
  be edited or do not exist.

> [!SOURCE-ERROR] The error code says the number must be greater than 1, while the source's message
> text for it says greater than 0. The spec does not resolve this; the behaviour notes describe the
> rule as "positive integers", so expect any ID of 1 or more to be accepted and verify if it matters.
<!-- source-error: N/manufacturing/productionCharges.updateChargesToCustomUnitCost | - | error_code_says_greater_than_1_message_0 -->

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2026.1

- The posting period must be open.

### productionCharges.updateChargesToItemPurchasePrice(options)
<!-- id: N/manufacturing/productionCharges.updateChargesToItemPurchasePrice -->

Updates only the listed charge lines to the item's current purchase price (where
`updateAllChargesToItemPurchasePrice` does every line). The same line kinds are eligible as for
`updateChargesToCustomUnitCost`: credit lines; non-inventory, service or other-charge items; items
for purchase or resale; and lines whose cost category is not outsourcing or landed-cost service.
Applies to assembly builds, work order completions and work order issues.

> [!SOURCE-ERROR] The source describes this function with text copied from
> `updateChargesToCustomUnitCost`, so its own description is unreliable. Its name, parameters and the
> source's sample point to "set chosen lines to the current purchase price", which is how it is
> described here.
<!-- source-error: N/manufacturing/productionCharges.updateChargesToItemPurchasePrice | - | description_same_as_custom_unit_cost_method -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.transactionId` | number | Yes | |
| `options.transactionLineIds` | number[] | Yes | |

**Returns:** `void`

**Errors:**
- `INVALID_NUMBER_MUST_BE_GREATER_THAN_1`: `options.transactionId` or a line ID is not a positive
  integer.
- `FEATURE_1_MUST_BE_ENABLED_TO_USE_2_API`: the assembly items feature is disabled.
- `PREFERENCE_1_REQUIRED_FOR_THIS_OPERATION`: the bulk cost update preference is off.
- `ACCESS_DENIED`: the role cannot edit manufacturing transactions.
- `TRANSACTION_1_IS_INVALID_OR_HAS_NO_EDITABLE_TRANSACTION_LINES`: the transaction is invalid or has
  no editable lines.
- `PROVIDED_TRANSACTION_LINES_ARE_NOT_EDITABLE_OR_INVALID_1`: one or more of the given lines cannot
  be edited or do not exist.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2026.1

- The posting period must be open.

## Examples

A scheduled script that refreshes every charge line on a list of recent assembly builds after the
purchasing team updated vendor prices. Each build is handled separately so one closed period does
not stop the rest.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/manufacturing/productionCharges', 'N/log'], (productionCharges, log) => {
  const execute = () => {
    const buildIds = [70412, 70418, 70433];
    buildIds.forEach((transactionId) => {
      try {
        productionCharges.updateAllChargesToItemPurchasePrice({ transactionId });
      } catch (e) {
        log.error({ title: `Build ${transactionId} not updated`, details: `${e.name}: ${e.message}` });
      }
    });
  };

  return { execute };
});
```

A RESTlet a costing tool can call to apply a negotiated outsourced-labour rate to two lines of a work
order completion, as a flat cost rather than per base unit:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/manufacturing/productionCharges'], (productionCharges) => {
  const post = (body) => {
    productionCharges.updateChargesToCustomUnitCost({
      transactionId: Number(body.completionId),
      transactionLineIds: body.lineIds.map(Number),
      newUnitCost: Number(body.unitCost),
      isUnitCostPerBaseUnit: false
    });
    return { updated: body.lineIds.length };
  };

  return { post };
});
```
