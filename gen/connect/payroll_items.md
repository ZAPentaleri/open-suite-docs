# Payroll_items (`payroll_items`)
<!-- id: connect:payroll_items -->

| Fact | Value |
|---|---|
| Table | `payroll_items` |
| Record | `payrollitem` |
| Primary key | `payroll_item_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `derived_from_payroll_item_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `source_payroll_item_for_derived_item` |
| `derived_rate_multiplier` | NUMBER |  | 15 | 8 |  |  | `purpose`: `derived_payroll_item_rate_multiplier` |
| `expense_account` | NUMBER |  | 39 | 0 |  |  | `purpose`: `expense_account` |
| `isinactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `inactive` |
| `liability_account` | NUMBER |  | 39 | 0 |  |  | `purpose`: `liability_account` |
| `limit_0` | NUMBER |  | 12 | 2 |  |  | `purpose`: `limit` |
| `locality` | VARCHAR2 | 15 |  |  |  |  | `purpose`: `locality` |
| `name` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `name` |
| `payroll_item_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `payroll_item_id` |
| `payroll_item_type_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `payroll_item_type_id` |
| `rate` | VARCHAR2 | 42 |  |  |  |  | `purpose`: `rate` |
| `state` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `state` |
| `tax_number` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `agency_id` |
| `vendor_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `vendor_id` |
| `w2_code` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `w_2_box` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Employee_time_payroll_items_fk` | `employee_time` | `payroll_item_id` | `payroll_item_id` |
| `Opportunity_lines_payroll_items_fk` | `opportunity_lines` | `payroll_item_id` | `payroll_item_id` |
