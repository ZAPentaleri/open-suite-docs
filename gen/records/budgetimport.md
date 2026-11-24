# Budget Import (`budgetimport`)
<!-- id: record:budgetimport -->

| Fact | Value |
|---|---|
| Internal ID | `budgetimport` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `accountingbook` | select | Accounting Book | yes |  | marked_new | `purpose`: `accounting_book_of_budget`; `requires_one_of`: `full_multi_book_accounting`, `adjustment_only_books` |
| `budgettype` | checkbox | Global | no |  | marked_new | `purpose`: `budget_category_type`; `requires_feature`: `Multiple Budgets` [`real_name`]; `allowed_values`: `local`, `global`; `option_meanings`: `local:subsidiary_level_local_currency`, `global:root_subsidiary_currency` (when `oneworld`) |
| `category` | select | Budget Category | yes |  | marked_new | `purpose`: `budget_category`; `use_when`: `several_budgets_same_segments`; `ui_effect`: `new_option_creates_record` |
| `class` | select | Class | no |  | marked_new | `purpose`: `budget_class`; `default_behavior`: `all_classs` (when `blank`) |
| `currency` | select | Currency | no |  | marked_new | `purpose`: `budget_currency`; `value_source`: `company_base_currency` (when `not_oneworld`); `value_source`: `root_parent_base_currency` (when `global_budget_category`); `value_source`: `local_subsidiary_base_currency` (when `local_budget_category`) |
| `customer` | select | Customer | no |  | marked_new | `purpose`: `budget_customer_or_project` |
| `department` | select | Department | no |  | marked_new | `purpose`: `budget_department`; `default_behavior`: `all_departments` (when `blank`) |
| `item` | select | Item | no |  | marked_new | `purpose`: `budget_item` |
| `location` | select | Location | no |  | marked_new | `purpose`: `budget_location`; `default_behavior`: `all_locations` (when `blank`) |
| `subsidiary` | select | Subsidiary | yes |  | marked_new | `purpose`: `budget_subsidiary` |
| `year` | select | Year | yes |  | marked_new | `purpose`: `budget_year` |
| `account` | select | Account | yes |  | marked_new |  |
| `amount` | currency | Amount | no |  | marked_new |  |
| `externalid` | text | External ID | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
| `periodamount1` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount10` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount11` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount12` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount13` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount14` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount15` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount16` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount17` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount18` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount19` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount2` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount20` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount21` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount22` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount23` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount24` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount3` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount4` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount5` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount6` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount7` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount8` | currency | Period {1} Amount | no |  | marked_new |  |
| `periodamount9` | currency | Period {1} Amount | no |  | marked_new |  |
| `yearnoaccountingperiod` | text | Year | yes |  | marked_new |  |
