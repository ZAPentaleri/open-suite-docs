# Revenue Recognition Event (`billingrevenueevent`)
<!-- id: record:billingrevenueevent -->

| Fact | Value |
|---|---|
| Internal ID | `billingrevenueevent` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| User event scripts | no |
| Tags | `custom_fields_supported`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `requires_feature`: `Advanced Revenue Management Essentials` [`real_name`]
- `user_events_supported`: `false`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `not_supported`: `copy`, `transform`
- `requires`: `revenue_recognition_event_type_exists`
- `not_scriptable`: `revenue_recognition_event_types`
- `purpose`: `triggers_revenue_recognition_plan_creation`
- `ui_location`: `lists_accounting_revenue_recognition_event_types_new` (when `creating_event_type`)
- `doc_inconsistency`: `scripting_level_typo_red_for_read` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amount` | currency | Amount (Foreign Currency) | no | no |  | `purpose`: `total_revenue_of_plan` |
| `cumulativepercentcomplete` | percent | Cumulative Percent Complete | no | no |  | `purpose`: `cumulative_percent_complete`; `required_when`: `rule_amount_source_event_percent_complete` |
| `eventdate` | date | Event Date | yes | no |  | `purpose`: `event_trigger_date` |
| `eventpurpose` | select | Event Purpose | yes | no |  | `purpose`: `event_purpose`; `allowed_values`: `Actual`, `Forecast`; `read_only_when`: `no_sales_contract_source` |
| `eventtype` | select | Event Type | yes | no |  | `purpose`: `revenue_event_type`; `constraint`: `matches_source_create_revenue_plans_on` |
| `quantity` | float | Quantity | no | no |  | `purpose`: `event_quantity`; `required_when`: `rule_amount_source_event_percent_quantity` |
| `record` | select | Custom Record | no | no |  | `purpose`: `custom_record_sales_contract_source`; `requires_together`: `recordtype`, `record`; `constraint`: `one_sales_contract_source_per_event`; `help_text_shared_with`: `billingrevenueevent.recordtype` |
| `recordtype` | select | Custom Record Type | no | no |  | `purpose`: `custom_record_sales_contract_source`; `requires_together`: `recordtype`, `record`; `constraint`: `one_sales_contract_source_per_event`; `help_text_shared_with`: `billingrevenueevent.record` |
| `transactionline` | select | Transaction Line | no | no |  | `purpose`: `transaction_line_sales_contract_source`; `constraint`: `one_sales_contract_source_per_event` |
| `customform` | select | Custom Form | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount (Foreign Currency) |  |
| `cumulativepercentcomplete` | percent | Cumulative Percent Complete |  |
| `enddate` | date | End Date |  |
| `eventdate` | date | Event Date |  |
| `eventpurpose` | select | Event Purpose |  |
| `eventtype` | select | Event Type |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `quantity` | float | Quantity |  |
| `record` | select | Custom Record |  |
| `recordtype` | select | Custom Record Type |  |
| `startdate` | date | Start Date |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount (Foreign Currency) |  |
| `cumulativepercentcomplete` | percent | Cumulative Percent Complete |  |
| `enddate` | date | End Date |  |
| `eventdate` | date | Event Date |  |
| `eventpurpose` | select | Event Purpose |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `quantity` | float | Quantity |  |
| `record` | select | Custom Record |  |
| `recordtype` | select | Custom Record Type |  |
| `startdate` | date | Start Date |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billingrevenueeventtype` | Revenue Recognition Event Type | Revenue Recognition Event Type |
| `subscriptionline` | Subscription Line | {Subscription} Line |
| `transaction` | Transaction | Transaction |

## Suspected source errors

> [!SOURCE-ERROR] `scripting_level_typo_red_for_read`
<!-- source-error: record:billingrevenueevent | - | scripting_level_typo_red_for_read -->
