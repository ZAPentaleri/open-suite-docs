# Subscription Change Order (`subscriptionchangeorder`)
<!-- id: record:subscriptionchangeorder -->

| Fact | Value |
|---|---|
| Internal ID | `subscriptionchangeorder` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `SuiteBilling` [`real_name`]
- `required_for`: `activating_draft_subscription`
- `requires`: `default_values_on_create`
- `purpose`: `transact_subscription_changes`
- `configured_by`: `enable_features_transactions_subtab` [`suitebilling`]
- `ui_location`: `transactions_subscriptions_view_subscription_change_orders`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billingaccount` | select | Billing Account | yes | no |  | `purpose`: `billing_account`; `value_kind`: `billing_schedule_terms_address_and_currency`; `can`: `customers_have_several_billing_accounts` |
| `customer` | select | Customer | yes | no |  | `purpose`: `subscription_customer`; `ui_effect`: `new_option_creates_record` |
| `effectivedate` | date | Effective Date | yes | no |  | `purpose`: `change_effective_date` |
| `idnumber` | text | Number | no | no |  | `purpose`: `subscription_number`; `value_source`: `system_generated` |
| `memo` | textarea | Memo | no | no |  | `purpose`: `memo` |
| `requestoffcycleinvoice` | checkbox | Request Off-Cycle Invoice For Advance Charges | no | no |  | `purpose`: `off_cycle_invoice_default`; `value_kind`: `bill_partial_period_early` |
| `requestor` | select | Requester | yes | no |  | `purpose`: `change_order_requestor` |
| `subscription` | select | Subscription | yes | no |  | `purpose`: `subscription`; `value_source`: `system_displayed` |
| `subscriptionplan` | select | Subscription Plan | no | no |  | `purpose`: `subscription_plan`; `ui_effect`: `plus_sign_creates_plan` |
| `approvaldate` | datetime | Date Approved | no | no |  |  |
| `approvalstatus` | select | Approval Status | yes | no |  |  |
| `createdby` | text | Created By | no | no |  |  |
| `datecreated` | datetime | Date Created | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `reactivationoption` | select | Reactivation Option | yes | no |  |  |
| `renewalenddate` | date | Renewal End Date | yes | no |  |  |
| `renewalmethod` | select | Renewal Method | yes | no |  |  |
| `renewalplan` | select | Renewal Subscription Plan | yes | no |  |  |
| `renewalpricebook` | select | Renewal Price Book | no | no |  |  |
| `renewalstartdate` | date | Renewal Start Date | yes | no |  |  |
| `renewalterm` | select | Renewal Term | yes | no |  |  |
| `renewaltrantype` | select | Renewal Transaction Type | no | no |  |  |
| `saascommitmenteffectivedate` | date | SaaS Metric Commitment Effective Date | no |  | marked_new |  |
| `saascommitmentenddate` | date | SaaS Metric Commitment End Date | no |  | marked_new |  |
| `saascommitmentstartdate` | date | SaaS Metric Commitment Start Date | no |  | marked_new |  |
| `subscriptiontermduration` | posinteger |  | no | no |  |  |
| `subscriptiontermtype` | text |  | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |

## Sublist `newsubline` (New Lines)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `item` | text |  | no |  |  |
| `subscriptionline` | text |  | no |  |  |

## Sublist `renewalsteps` (Renewal Steps)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `subscription` | integer |  | no |  |  |
| `transaction` | integer |  | no |  |  |

## Sublist `subline` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `apply` | checkbox | Include | no |  |  |
| `billingmode` | text |  | no |  |  |
| `discount` | ratehighprecision |  | no |  |  |
| `discountnew` | ratehighprecision | New Discount | no |  |  |
| `enddate` | date |  | no |  |  |
| `includeinrenewal` | text |  | no |  |  |
| `item` | integer |  | no |  |  |
| `priceplannew` | select | New Price Plan | no |  |  |
| `quantity` | nonnegfloat |  | no |  |  |
| `quantitynew` | nonnegfloat | New Quantity | no |  |  |
| `recurringamount` | currency | Recurring Amount | no |  |  |
| `recurringamountnew` | currency | New Recurring Amount | no |  |  |
| `sequence` | text | Line Number | no |  |  |
| `status` | text |  | no |  |  |
| `statusnew` | select | New Status | no |  |  |
| `subscriptionline` | text |  | no |  |  |
| `terminationdate` | date |  | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `action` | select | Action |  |
| `approvaldate` | datetime | Date Approved |  |
| `approvalstatus` | select | Approval Status |  |
| `billingaccount` | select | Billing Account |  |
| `customer` | select | Customer |  |
| `datecreated` | datetime | Date Created |  |
| `effectivedate` | date | Effective Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `idnumber` | text | Number |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `memo` | text | Memo |  |
| `newenddate` | date | New End Date |  |
| `orderedeffectivedate` | date | Ordered Effective Date |  |
| `renewalenddate` | date | Renewal End Date |  |
| `renewalmethod` | select | Renewal Method |  |
| `renewalstartdate` | date | Renewal Start Date |  |
| `renewalterm` | select | Renewal Term |  |
| `renewaltrantype` | select | Renewal Transaction Type |  |
| `requestoffcycleinvoice` | checkbox | Request Off-Cycle Invoice for Advance Charges |  |
| `requestor` | select | Requestor |  |
| `subscription` | select | Subscription |  |
| `subscriptionplan` | select | Subscription Plan |  |
| `subsidiary` | select | Subsidiary |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `action` | select | Action |  |
| `approvaldate` | datetime | Date Approved |  |
| `approvalstatus` | select | Approval Status |  |
| `billingaccount` | select | Billing Account |  |
| `customer` | select | Customer |  |
| `datecreated` | datetime | Date Created |  |
| `effectivedate` | date | Effective Date |  |
| `externalid` | select | External ID |  |
| `idnumber` | text | Number |  |
| `internalid` | select | Internal ID |  |
| `memo` | text | Memo |  |
| `newenddate` | date | New End Date |  |
| `renewalenddate` | date | Renewal End Date |  |
| `renewalmethod` | select | Renewal Method |  |
| `renewalstartdate` | date | Renewal Start Date |  |
| `renewalterm` | select | Renewal Term |  |
| `renewaltrantype` | select | Renewal Transaction Type |  |
| `requestoffcycleinvoice` | checkbox | Request Off-Cycle Invoice for Advance Charges |  |
| `requestor` | select | Requestor |  |
| `subscription` | select | Subscription |  |
| `subscriptionplan` | select | Subscription Plan |  |
| `subsidiary` | select | Subsidiary |  |
| `view` | text | View |  |
