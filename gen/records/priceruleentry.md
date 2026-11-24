# Price Rule Entry (`priceruleentry`)
<!-- id: record:priceruleentry -->

| Fact | Value |
|---|---|
| Internal ID | `priceruleentry` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | no |
| Tags | `no_search`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Advanced Pricing` [`real_name`]
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `subrecords`: `quantityBreaks`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`
- `not_supported`: `search`, `transform`
- `not_updatable_fields`: `item`, `pricelist`
- `cannot`: `reference_inactive_price_rule`
- `purpose`: `automatic_price_adjustments`
- `value_examples`: `discount_at_ordered_quantity`
- `doc_inconsistency`: `page_calls_it_purchase_price_rule_entry`

## Suspected source errors

> [!SOURCE-ERROR] `page_calls_it_purchase_price_rule_entry`
<!-- source-error: record:priceruleentry | - | page_calls_it_purchase_price_rule_entry -->
