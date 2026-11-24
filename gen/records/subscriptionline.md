# Subscription Line (`subscriptionline`)
<!-- id: record:subscriptionline -->

| Fact | Value |
|---|---|
| Internal ID | `subscriptionline` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `custom_fields_supported`, `no_create`, `no_delete` |

## Record facts

- `script_types`: `client`, `server`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `not_supported`: `create`, `delete`
- `purpose`: `subscription_item_information`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `billingaccountstartdate` | date |  | no | no |  |  | `purpose`: `billing_account_start_date` |
| `catalogtype` | select | Catalog Type | yes |  |  |  | `purpose`: `catalog_type`; `allowed_values`: `Required`, `Optional`, `Add-On`; `option_meanings`: `required:must_be_included`, `optional:may_be_included`, `add_on:addable_at_sale_or_during_term` |
| `currency` | select |  | no | no |  |  | `purpose`: `subscription_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`] |
| `enddate` | date | End Date | no | no |  |  | `purpose`: `line_end_date` |
| `externalid` | text | ExternalId | no | no | yes |  | `purpose`: `external_id`; `value_source`: `system_generated` |
| `item` | select | Item | no | no |  |  | `purpose`: `line_item`; `value_source`: `subscription_selected` |
| `linenumber` | integer | Line Number | yes | no |  |  | `purpose`: `line_number`; `constraint`: `unique_integer` |
| `salesorder` | select | Sales Order | no | no |  |  | `purpose`: `sales_order`; `value_source`: `system_displayed` |
| `salesorderlinenumber` | integer | Sales Order Line Number | no | no |  |  | `purpose`: `sales_order_line_number`; `value_source`: `system_displayed` |
| `startdate` | date | Start Date | yes | no |  |  | `purpose`: `line_start_date` |
| `subscription` | select | Subscription | yes | no |  |  | `purpose`: `subscription`; `value_source`: `system_displayed` |
| `subscriptionlinestatus` | select | Status | yes | no |  |  | `purpose`: `line_status`; `value_examples`: `Draft`, `Active`, `Closed` |
| `subscriptionlinetype` | select | Type | no | no |  |  | `purpose`: `line_type`; `value_examples`: `One Time`, `Recurring`, `Usage` |
| `subscriptionplan` | select | Subscription Plan | yes | no |  |  | `purpose`: `subscription_plan`; `ui_effect`: `plus_sign_creates_plan` |
| `advancerenewalperiodnumber` | integer |  | no | no |  |  |  |
| `advancerenewalperiodunit` | select |  | no | no |  |  |  |
| `billingmode` | select | Billing Mode | no | no |  |  |  |
| `class` | select | Class | no |  |  |  |  |
| `customform` | select | Custom Form | yes | no |  |  |  |
| `department` | select | Department | no |  |  |  |  |
| `includeinrenewal` | checkbox | Include In Renewal Subscription | no | no |  |  |  |
| `location` | select | Location | no |  |  |  |  |
| `ponumber` | text | PO # | no | no |  |  |  |
| `prorateenddate` | checkbox | Prorate End Date | no | no |  |  |  |
| `proratestartdate` | checkbox | Prorate Start Date | no | no |  |  |  |
| `recurrencestartdate` | date | Recurrence Start Date | no | no |  |  |  |
| `saascommitmentenddate` | date | SaaS Metric Commitment End Date | no |  |  | marked_new |  |
| `saascommitmentstartdate` | date | SaaS Metric Commitment Start Date | no |  |  | marked_new |  |
| `subscriptionplanline` | select | Subscription Plan Line | yes | no |  |  |  |
| `terminationdate` | date | Termination Date | no | no |  |  |  |
| `total` | currency | Total | no | no |  |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `billingmode` | select | Billing Mode |  |
| `class` | select | Class |  |
| `department` | select | Department |  |
| `discount` | float | Discount |  |
| `enddate` | date | End Date |  |
| `estimatedrevrecenddate` | date | Estimated Revenue Recognition End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `flatratediscount` | float | Flat Rate Discount |  |
| `includeinrenewal` | checkbox | Include In Renewal Subscription |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `item` | select | Item |  |
| `linenumber` | integer | Line Number |  |
| `location` | select | Location |  |
| `prorateenddate` | checkbox | Prorate End Date |  |
| `proratestartdate` | checkbox | Prorate Start Date |  |
| `quantity` | float | Quantity |  |
| `recurrencestartdate` | date | Recurrence Start Date |  |
| `revrecoption` | select | Revenue Recognition Option |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subscription` | select | Subscription |  |
| `subscriptionlinetype` | select | Subscription Line Type |  |
| `subscriptionplan` | select | Subscription Plan |  |
| `subscriptionplanline` | select | Subscription Plan Line |  |
| `terminationdate` | date | Termination Date |  |
| `total` | currency | Total |  |
| `usagemultiplierline` | select | Included Quantity Multiplier Line |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `billingmode` | select | Billing Mode |  |
| `class` | select | Class |  |
| `department` | select | Department |  |
| `discount` | currency2 | Discount |  |
| `enddate` | date | End Date |  |
| `estimatedrevrecenddate` | date | Estimated Revenue Recognition End Date |  |
| `externalid` | select | External ID |  |
| `includeinrenewal` | checkbox | Include In Renewal Subscription |  |
| `internalid` | select | Internal ID |  |
| `item` | select | Item |  |
| `linenumber` | integer | Line Number |  |
| `location` | select | Location |  |
| `prorateenddate` | checkbox | Prorate End Date |  |
| `proratestartdate` | checkbox | Prorate Start Date |  |
| `quantity` | float | Quantity |  |
| `recurrencestartdate` | date | Recurrence Start Date |  |
| `revrecoption` | select | Revenue Recognition Option |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subscription` | select | Subscription |  |
| `subscriptionlinetype` | select | Subscription Line Type |  |
| `subscriptionplan` | select | Subscription Plan |  |
| `subscriptionplanline` | select | Subscription Plan Line |  |
| `subscriptionplanlineid` | integer | Subscription Plan Line Id |  |
| `terminationdate` | date | Termination Date |  |
| `total` | currency | Total |  |
| `usagemultiplierline` | select | Included Quantity Multiplier Line |  |
| `view` | text | View |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `includedusagemultiplierline` | Included Quantity Multiplier Line | Subscription Line |
| `item` | Item | Item |
| `subscription` | Subscription | Subscription |
| `transaction` | Transaction | Transaction |
