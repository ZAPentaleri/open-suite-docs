# Charges (`charges`)
<!-- id: connect:charges -->

| Fact | Value |
|---|---|
| Table | `charges` |
| Record | `charge` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `amount` | NUMBER |  | 20 | 2 |  |  | `purpose`: `amount_of_the_charges` |
| `billing_account_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `billing_account_id` |
| `billing_mode_id` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_billing_mode` |
| `billing_schedule_id` | NUMBER |  | 39 | 0 | `billing_schedule_descriptions`.`billing_schedule_id` | foreign_key | `purpose`: `charge_billing_schedule` |
| `category_0` | NUMBER |  | 39 | 0 |  |  | `purpose`: `category_of_the_charges` |
| `charge_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `charge_external_id` |
| `charge_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_id`; `doc_inconsistency`: `says_changes_for_charges` |
| `chargeemployee` | NUMBER |  | 39 | 0 |  |  | `purpose`: `employee_of_the_charges` |
| `chargerule` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_rule` |
| `chargestage` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_stage` |
| `chargetype` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_type` |
| `chargeuse` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `charge_use` |
| `class_0` | NUMBER |  | 39 | 0 |  |  | `purpose`: `class_of_the_charges` |
| `currency` | NUMBER |  | 39 | 0 |  |  | `purpose`: `currency_of_the_charges` |
| `customer` | NUMBER |  | 39 | 0 |  |  | `purpose`: `customer_of_the_charges` |
| `date_0` | TIMESTAMP |  |  |  |  |  | `purpose`: `charge_date`; `time_zone`: `gmt` |
| `date_bill` | TIMESTAMP |  |  |  |  |  | `purpose`: `charge_bill_date`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `last_modified_date`; `time_zone`: `gmt` |
| `date_service_end` | TIMESTAMP |  |  |  |  |  | `purpose`: `charge_service_end_date`; `time_zone`: `gmt` |
| `date_service_start` | TIMESTAMP |  |  |  |  |  | `purpose`: `charge_service_start_date`; `time_zone`: `gmt` |
| `department` | NUMBER |  | 39 | 0 |  |  | `purpose`: `department_of_the_charges` |
| `description` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `description_of_the_charges` |
| `group_order` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_group_order` |
| `item` | NUMBER |  | 39 | 0 |  |  | `purpose`: `item_of_the_charges` |
| `location_0` | NUMBER |  | 39 | 0 |  |  | `purpose`: `location` |
| `memo` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `memo_of_the_charges` |
| `quantity` | NUMBER |  | 18 | 8 |  |  | `purpose`: `quantity_of_the_charges` |
| `rate` | NUMBER |  | 30 | 15 |  |  | `purpose`: `rate_of_the_charges` |
| `run_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_run_id` |
| `sales_order` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_sales_order` |
| `so_line` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_sales_order_line` |
| `subscription_line_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subscription_line_id` |
| `subsidiary_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_subsidiary` |
| `time_source` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_time_source` |
| `unit_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `charge_unit` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `charges_billing_schedule_descriptions_fk` | `billing_schedule_id` | `billing_schedule_descriptions` | `billing_schedule_id` |

## Suspected source errors

> [!SOURCE-ERROR] `says_changes_for_charges` (on `charge_id`)
<!-- source-error: connect:charges | charge_id | says_changes_for_charges -->
