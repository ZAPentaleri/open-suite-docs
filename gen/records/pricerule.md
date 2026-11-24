# Price Rule (`pricerule`)
<!-- id: record:pricerule -->

| Fact | Value |
|---|---|
| Internal ID | `pricerule` |
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
- `subrecords`: `customerdimension`
- `script_functions`: `read`, `create`, `update`, `copy`, `delete`
- `not_supported`: `search`, `transform`
- `rule`: `base_and_creating_price_rule_must_be_active`
- `rule`: `one_purchase_price_per_item`
- `purpose`: `set_prices_for_groups_of_items`
- `value_examples`: `discount_for_upcoming_sale_event`
