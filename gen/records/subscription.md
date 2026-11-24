# Subscription (`subscription`)
<!-- id: record:subscription -->

| Fact | Value |
|---|---|
| Internal ID | `subscription` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `purpose`: `recurring_subscription`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]
- `ui_location`: `transactions_subscriptions_create_subscriptions`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `autorenewal` | checkbox | Automatically Initiate Renewal Process | no | no |  | `purpose`: `auto_renewal`; `requires`: `renewal_lead_date` |
| `billingaccount` | select | Billing Account | yes | no |  | `purpose`: `billing_account`; `value_kind`: `billing_schedule_terms_address_and_currency`; `can`: `customers_have_several_billing_accounts` |
| `billingaccountstartdate` | date | Billing Account Start Date | yes | no |  | `purpose`: `billing_account_start_date` |
| `billingschedule` | select | Billing Schedule | yes | no |  | `purpose`: `billing_schedule`; `value_source`: `system_displayed` |
| `billingsubscriptionstatus` | select | Status | no | no |  | `purpose`: `subscription_status`; `value_source`: `system_displayed` |
| `class` | select | Class | no |  |  | `purpose`: `class`; `caveat`: `optional_field` |
| `currency` | select | Currency | no | no |  | `purpose`: `subscription_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`] |
| `customer` | select | Customer | yes | no |  | `purpose`: `subscription_customer`; `ui_effect`: `new_option_creates_record` |
| `defaultrenewalmethod` | select | Default Renewal Method | yes | no |  | `purpose`: `default_renewal_method`; `options`: `Extending Existing Subscription`, `Create New Subscription`; `option_meanings`: `extend:keeps_plan_price_book_and_type`, `create_new:choose_renewal_plan_price_book_and_type` |
| `defaultrenewalplan` | select | Default Renewal Subscription Plan | yes | no |  | `purpose`: `default_renewal_plan`; `ui_effect`: `new_option_creates_record` |
| `defaultrenewalpricebook` | select | Default Renewal Price Book | no | no |  | `purpose`: `default_renewal_price_book` |
| `defaultrenewalterm` | select | Default Renewal Term | yes | no |  | `purpose`: `default_renewal_term`; `ui_effect`: `new_option_creates_record` |
| `defaultrenewaltrantype` | select | Default Renewal Transaction Type | no | no |  | `purpose`: `renewal_transaction_type`; `allowed_values`: `estimate`, `opportunity`, `sales_order`, `no_transaction` |
| `enddate` | date | End Date | no | no |  | `purpose`: `subscription_end_date`; `value_source`: `start_plus_term` (when `standard_term`); `required_when`: `custom_term` |
| `estimatedrevrecenddate` | date | Estimated Revenue Recognition End Date | no |  |  | `purpose`: `estimated_rev_rec_end_date`; `applies_when`: `evergreen_term`; `default_behavior`: `3_years_after_activation` (when `blank`); `effect`: `periodically_updated` |
| `frequency` | select | Billing Frequency | yes | no |  | `purpose`: `billing_frequency`; `value_source`: `billing_schedule` |
| `idnumber` | text | Subscription ID | no | no |  | `purpose`: `subscription_number`; `value_source`: `system_generated` |
| `initialterm` | select | Initial Term | yes | no |  | `purpose`: `initial_term`; `ui_effect`: `new_option_creates_record` |
| `lastbillcycledate` | date | Last Bill Cycle Date | no | no |  | `purpose`: `next_billing_cycle_start`; `doc_inconsistency`: `last_bill_cycle_field_described_as_next` |
| `lastbilldate` | date | Last Bill Date | no | no |  | `purpose`: `last_invoice_date`; `value_source`: `system_displayed` |
| `name` | text | Subscription Name | yes | no |  | `purpose`: `subscription_name`; `default_behavior`: `auto_generated_on_save` (when `blank`) |
| `nextbillcycledate` | date | Next Bill Cycle Date | no | no |  | `purpose`: `next_bill_date`; `value_source`: `system_displayed` |
| `nextrenewalstartdate` | date | Next Renewal Start Date | no | no |  | `purpose`: `next_renewal_start_date`; `value_source`: `system_displayed` |
| `pricebook` | select | Price Book | no | no |  | `purpose`: `price_book` |
| `salesorder` | select | Originating Sales Order | no | no |  | `purpose`: `sales_order_id`; `value_source`: `system_displayed` |
| `startdate` | date | Start Date | no | no |  | `purpose`: `subscription_start_date`; `default_behavior`: `current_date` (when `blank`); `effect`: `earliest_line_activation_date`; `editable_only_when`: `no_line_active` |
| `subscriptionplan` | select | Subscription Plan | no | no |  | `purpose`: `subscription_plan`; `ui_effect`: `plus_sign_creates_plan` |
| `subscriptionrevision` | integer | Subscription Revision | no | no |  | `purpose`: `revision_number`; `default_value`: `1` (when `first_activation`); `effect`: `incremented_on_change_order_save` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `doc_inconsistency`: `help_describes_catalog_type` |
| `advancerenewalperiodnumber` | integer |  | yes | no |  |  |
| `advancerenewalperiodunit` | select |  | yes | no |  |  |
| `autoname` | checkbox | Auto | no |  |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `department` | select | Department | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `initialtermduration` | posinteger |  | no |  |  |  |
| `initialtermtype` | select |  | no |  |  |  |
| `initialtermunits` | select |  | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `location` | select | Location | no |  |  |  |
| `otherrecordnumber` | text |  | no |  |  |  |
| `renewalnumber` | integer | Number of Renewal | no | no |  |  |
| `subscriptionplanname` | text | Subscription Plan | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Sublist `priceinterval` (Price Book Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `catalogtype` | select | Catalog Type | yes |  |  |
| `chargetype` | select | Subscription Line Type | no |  |  |
| `discount` | ratehighprecision | Discount | no |  |  |
| `frequency` | select | Charge Frequency | yes |  |  |
| `id` | identifier |  | no |  |  |
| `includedquantity` | posfloat | Included Quantity | no |  |  |
| `item` | select | Item | no |  |  |
| `linenumber` | integer | Line Number | no |  |  |
| `multiplierline` | select | Included Quantity Multiplier | no |  |  |
| `priceplan` | text | Price Plan | no |  |  |
| `prorateby` | select | Prorate By | no |  |  |
| `quantity` | posfloat | Quantity | no |  |  |
| `recurringamount` | currency | Recurring Amount | no |  |  |
| `repeatevery` | select | Repeat Every | yes |  |  |
| `startdate` | date | Start Date | yes |  |  |
| `startoffsetunit` | select | Interval | no |  |  |
| `startoffsetvalue` | integer | Start On | no |  |  |
| `status` | text |  | no |  |  |
| `subscriptionplanline` | integer | Subscription Plan Line | no |  |  |

## Sublist `sublinefromplan` (Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | text |  | no |  |  |
| `revrecoption` | text |  | no |  |  |

## Sublist `subscriptionline` (Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billingmode` | select | Billing Mode | no |  |  |
| `catalogtype` | select | Catalog Type | yes |  |  |
| `class` | select | Class | no |  |  |
| `department` | select | Department | no |  |  |
| `discount` | percent | Discount | no |  |  |
| `enddate` | date | End Date | no |  |  |
| `includeinrenewal` | checkbox | Include In Renewal Subscription | no |  |  |
| `isincluded` | checkbox | Include | no |  |  |
| `item` | select | Item | no |  |  |
| `linenumber` | text | Line Number | no |  |  |
| `location` | select | Location | no |  |  |
| `prorateenddate` | checkbox | Prorate End Date | no |  |  |
| `proratestartdate` | checkbox | Prorate Start Date | no |  |  |
| `quantity` | posfloat | Quantity | no |  |  |
| `recurrencestartdate` | date | Recurrence Start Date | no |  |  |
| `revrecoption` | text | Revenue Recognition Option | no |  |  |
| `startdate` | date | Start Date | no |  |  |
| `status` | select | Status | yes |  |  |
| `subscription` | select | Subscription | no |  |  |
| `subscriptionline` | integer |  | no |  |  |
| `subscriptionlinetype` | select | Subscription Line Type | no |  |  |
| `terminationdate` | date | Termination Date | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `advancerenewalperiodnumber` | integer | Advance Renewal Period |  |
| `advancerenewalperiodunit` | select | Advance Renewal Period Unit |  |
| `autorenewal` | checkbox | Automatically Initiate Renewal Process |  |
| `billingaccount` | select | Billing Account |  |
| `class` | select | Class |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `datecreated` | datetime | Date Created |  |
| `defaultrenewalmethod` | select | Default Renewal Method |  |
| `defaultrenewalterm` | select | Default Renewal Term |  |
| `defaultrenewaltrantype` | select | Default Transaction Type |  |
| `department` | select | Department |  |
| `enddate` | date | End Date |  |
| `estimatedrevrecenddate` | date | Estimated Revenue Recognition End Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `initialterm` | select | Initial Term |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `nextrenewalstartdate` | date | Next Renewal Start Date |  |
| `originalsalestransaction` | select | Original Sales Transaction |  |
| `parentsubscriptionid` | integer | Parent Subscription ID |  |
| `renewalsubscriptionname` | select | Renewal Subscription Name |  |
| `renewaltransaction` | select | Renewal Transaction |  |
| `rootsubscriptionid` | integer | Root Subscription ID |  |
| `salesorder` | select | Sales Order |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subscriptionplan` | select | Subscription Plan |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `advancerenewalperiodnumber` | integer | Advance Renewal Period |  |
| `advancerenewalperiodunit` | select | Advance Renewal Period Unit |  |
| `autorenewal` | checkbox | Automatically Initiate Renewal Process |  |
| `billingaccount` | select | Billing Account |  |
| `class` | select | Class |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `datecreated` | datetime | Date Created |  |
| `defaultrenewalmethod` | select | Default Renewal Method |  |
| `defaultrenewalterm` | select | Default Renewal Term |  |
| `defaultrenewaltrantype` | select | Default Transaction Type |  |
| `department` | select | Department |  |
| `enddate` | date | End Date |  |
| `estimatedrevrecenddate` | date | Estimated Revenue Recognition End Date |  |
| `externalid` | select | External ID |  |
| `initialterm` | select | Initial Term |  |
| `internalid` | select | Internal ID |  |
| `location` | select | Location |  |
| `name` | text | Name |  |
| `nextrenewalstartdate` | date | Next Renewal Start Date |  |
| `originalsalestransaction` | text | Original Sales Transaction |  |
| `renewalnumber` | integer | Renewal Sequence |  |
| `renewalsubscriptionname` | text | Renewal Subscription Name |  |
| `renewaltransaction` | select | Renewal Transaction |  |
| `salesorder` | select | Sales Order |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subscriptionplan` | select | Subscription Plan |  |
| `subsidiary` | select | Subsidiary |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `billingaccount` | Billing Account | Billing Account |
| `currency` | Currency | Currency |
| `customer` | Customer | Customer |
| `item` | Subscription Plan | Item |

## Suspected source errors

> [!SOURCE-ERROR] `last_bill_cycle_field_described_as_next` (on `lastbillcycledate`)
<!-- source-error: record:subscription | lastbillcycledate | last_bill_cycle_field_described_as_next -->

> [!SOURCE-ERROR] `help_describes_catalog_type` (on `subsidiary`)
<!-- source-error: record:subscription | subsidiary | help_describes_catalog_type -->
