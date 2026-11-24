# Subscription Plan (`subscriptionplan`)
<!-- id: record:subscriptionplan -->

| Fact | Value |
|---|---|
| Internal ID | `subscriptionplan` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Item |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `purpose`: `information_template_for_subscriptions`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]
- `ui_location`: `lists_subscriptions_subscription_plans`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `autorenewal` | checkbox | Automatically Initiate Renewal Process | no | no |  | `purpose`: `auto_renewal`; `effect`: `renewal_starts_before_end_date_offset` |
| `class` | select | Class | no | yes |  | `purpose`: `class`; `max_length`: `31` characters; `ui_effect`: `new_option_creates_record`; `doc_inconsistency`: `class_help_says_department` |
| `defaultrenewalmethod` | select | Default Renewal Method | yes | no |  | `purpose`: `default_renewal_method`; `options`: `Extend Existing Subscription`, `Create new subscription` |
| `defaultrenewalplan` | select | Default Renewal Subscription Plan | no | no |  | `purpose`: `default_renewal_plan`; `ui_effect`: `new_option_creates_record` |
| `defaultrenewalterm` | select | Default Renewal Term | yes | no |  | `purpose`: `default_renewal_term` |
| `defaultrenewaltrantype` | select | Default Renewal Transaction Type | no | no |  | `purpose`: `renewal_transaction_type` |
| `department` | select | Department | no | yes |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record` |
| `description` | textarea | Description | no | no |  | `purpose`: `display_code`; `caveat`: `optional_field`; `doc_inconsistency`: `description_help_describes_display_name` |
| `displayname` | text | Display Name/Code | no | no |  | `purpose`: `display_name`; `caveat`: `optional_field` |
| `includechildren` | checkbox | Include Children | no | no |  | `purpose`: `include_children` |
| `incomeaccount` | select | Income Account | no | no |  | `purpose`: `income_account` |
| `initialterm` | select | Initial Term | yes | no |  | `purpose`: `initial_term`; `value_examples`: `6 months`, `1 year`; `caveat`: `unused_after_activation_end_date_governs` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_plan` |
| `itemid` | text | Subscription Plan Name | yes | no |  | `purpose`: `plan_name` |
| `location` | select | Location | no | yes |  | `purpose`: `transaction_location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record` |
| `subsidiary` | select | Subsidiary | no | no |  | `purpose`: `subsidiary`; `caveat`: `optional_field` |
| `advancerenewalperiodnumber` | integer |  | yes | no |  |  |
| `advancerenewalperiodunit` | select |  | yes | no |  |  |
| `createddate` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `itemtype` | text |  | no | no |  |  |
| `lastmodifieddate` | datetime | Last Modified | no | no |  |  |

## Sublist `member` (Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billingmode` | select | Billing Mode | yes |  |  |
| `isrequired` | checkbox | Required | no |  |  |
| `item` | select | Item | yes |  |  |
| `lineid` | integer | Line Id | no |  |  |
| `linenumber` | integer | Line Number | no |  |  |
| `prorateenddate` | checkbox | Prorate End Date | no |  |  |
| `proratestartdate` | checkbox | Prorate Start Date | no |  |  |
| `renewaloption` | select | Include In Renewal Subscription | yes |  |  |
| `subscriptionlinetype` | select | Type | yes |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `class_help_says_department` (on `class`)
<!-- source-error: record:subscriptionplan | class | class_help_says_department -->

> [!SOURCE-ERROR] `description_help_describes_display_name` (on `description`)
<!-- source-error: record:subscriptionplan | description | description_help_describes_display_name -->
