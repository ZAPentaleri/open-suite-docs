# Payroll Batch Employee (`payrollbatchaddemployees`)
<!-- id: record:payrollbatchaddemployees -->

| Fact | Value |
|---|---|
| Internal ID | `payrollbatchaddemployees` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `supports_read`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `read`, `edit`
- `cannot`: `remove_employee_after_submit`
- `purpose`: `add_employees_to_payroll_batch`
- `requires`: `suitepeople_payroll_customer`
- `workaround`: `remove_employee_from_batch_in_ui`
