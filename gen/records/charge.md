# Charge (`charge`)
<!-- id: record:charge -->

| Fact | Value |
|---|---|
| Internal ID | `charge` |
| Category | transaction |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Charge-Based Billing` [`real_name`]
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `purpose`: `single_billable_amount_owed_by_client`
- `configured_by`: `enable_features_transactions_subtab` [`charge_based_billing`]
- `ui_location`: `transactions_customers_create_charges_list`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `amount` | poscurrency | Amount | yes | no |  | `purpose`: `charge_amount`; `value_source`: `rate_times_quantity`; `can`: `override` |
| `billingitem` | select | Item | no | no |  | `purpose`: `service_item_of_charge`; `effect`: `groups_project_charges_on_invoices`; `caveat`: `item_price_used_only_if_rule_bases_rate_on_it` |
| `billto` | select | Customer:Project | yes | no |  | `purpose`: `bill_to_project_or_customer` |
| `chargedate` | date | Date | yes | no |  | `purpose`: `charge_date` |
| `chargetype` | select | Charge Type | yes | no |  | `purpose`: `charge_generation_type`; `value_source`: `system_displayed` |
| `class` | select | Class | no | no |  | `purpose`: `class`; `value_source`: `system_displayed` |
| `createddate` | datetime | Date Created | no | no |  | `purpose`: `date_created`; `value_source`: `manual_entry_or_charge_run` |
| `currency` | select | Currency | no | no |  | `purpose`: `charge_currency`; `value_source`: `system_displayed` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `value_source`: `system_displayed` |
| `description` | text | Description | no | no |  | `purpose`: `charge_description` |
| `invoice` | select | Invoice | no | no |  | `purpose`: `billing_invoice`; `cannot`: `edit` (when `billed`) |
| `invoiceline` | text | Invoice Line | no | no |  | `purpose`: `billing_invoice_line`; `value_source`: `system_displayed` |
| `location` | select | Location | no | no |  | `purpose`: `location`; `value_source`: `system_displayed` |
| `quantity` | float | Quantity | yes | no |  | `purpose`: `charge_quantity`; `default_value`: `1` (when `project_charge`) |
| `rate` | rate | Rate | yes | no |  | `purpose`: `charge_rate`; `value_source`: `charge_rules` (when `project_charge`) |
| `rule` | select | Charge Rule | no | no |  | `purpose`: `generating_charge_rule`; `value_source`: `system_displayed` |
| `salesorder` | select | Sales Order | no | no |  | `purpose`: `sales_order`; `value_source`: `system_displayed` |
| `salesorderline` | select | Sales Order Line | no | no |  | `purpose`: `sales_order_line`; `value_source`: `system_displayed` |
| `stage` | select | Charge Stage | yes | no |  | `purpose`: `charge_stage`; `allowed_values`: `Ready`, `Hold`, `Processed`; `option_meanings`: `ready:required_to_bill`, `hold:needs_approval`, `processed:billed` |
| `timerecord` | select | Time Entry | no | no |  | `purpose`: `charged_time_record`; `value_source`: `system_displayed` |
| `use` | select | Charge Use | no | no |  | `purpose`: `charge_use`; `allowed_values`: `Actual`, `Forecast`; `option_meanings`: `actual:billable_when_ready`, `forecast:predicted_over_project_life` |
| `billingaccount` | select | Billing Account | no | no |  |  |
| `projecttask` | select | Project Task | no | no |  |  |
| `runid` | text | Charge Run ID | no | no |  |  |
| `subscriptionline` | select | Subscription Line | no | no |  |  |
| `transaction` | select | Expense Source | no | no |  |  |
| `transactionline` | select | Expense Line | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `historytab` | System Notes |
| `source` | Source |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount |  |
| `billingaccount` | select | Billing Account |  |
| `billingitem` | select | Billing Item |  |
| `billto` | select | Customer:Project |  |
| `chargedate` | date | Date |  |
| `chargetype` | select | Charge Type |  |
| `class` | select | Class |  |
| `createddate` | date | Date Created |  |
| `currency` | select | Currency |  |
| `department` | select | Department |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | Charge ID |  |
| `location` | select | Location |  |
| `modifieddate` | date | Date Modified |  |
| `quantity` | float | Quantity |  |
| `rate` | currency | Rate |  |
| `rule` | select | Charge Rule |  |
| `runid` | text | Charge Run ID |  |
| `salesorder` | integer | Sales Order |  |
| `stage` | select | Stage |  |
| `subscriptionline` | select | Subscription Line |  |
| `use` | select | Charge Use |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Amount |  |
| `billingaccount` | select | Billing Account |  |
| `billingitem` | select | Billing Item |  |
| `billto` | select | Customer:Project |  |
| `chargedate` | date | Date |  |
| `chargetype` | text | Charge Type |  |
| `class` | select | Class |  |
| `createddate` | date | Date Created |  |
| `currency` | select | Currency |  |
| `department` | select | Department |  |
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `id` | integer | Charge ID |  |
| `location` | select | Location |  |
| `modifieddate` | date | Date Modified |  |
| `quantity` | text | Quantity |  |
| `rate` | text | Rate |  |
| `rule` | select | Charge Rule |  |
| `runid` | text | Charge Run ID |  |
| `salesorder` | text | Sales Order Id |  |
| `stage` | text | Charge Stage |  |
| `subscriptionline` | select | Subscription Line |  |
| `use` | text | Charge Use |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `invoice` | Invoice | Transaction |
| `job` | Job | Job |
| `salesorder` | Sales Order | Transaction |
| `transaction` | Transaction | Transaction |
| `user` | User | Employee |
