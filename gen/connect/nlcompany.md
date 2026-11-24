# Nlcompany (`nlcompany`)
<!-- id: connect:nlcompany -->

| Fact | Value |
|---|---|
| Table | `nlcompany` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address` | VARCHAR2 | 2997 |  |  |  |  | `purpose`: `address` |
| `address_1` | VARCHAR2 | 450 |  |  |  |  | `purpose`: `address_line_1` |
| `address_2` | VARCHAR2 | 450 |  |  |  |  | `purpose`: `address_line_2` |
| `address_3` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_line_3` |
| `city` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `city` |
| `company_name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `company_name` |
| `country` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `country` |
| `e_mail` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `fax` | VARCHAR2 | 21 |  |  |  |  | `purpose`: `fax` |
| `federal_ein` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `employee_identification_number_ein` |
| `frst_fiscal_month` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `first_fiscal_month` |
| `legal_name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `legal_name` |
| `logo_url` | VARCHAR2 | 80 |  |  |  |  | `purpose`: `company_logo` |
| `parent_comp_id` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `parent_company_id` |
| `parent_company` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `parent_company` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `return_address` | VARCHAR2 | 2997 |  |  |  |  | `purpose`: `address_for_returns` |
| `shipping_address` | VARCHAR2 | 2997 |  |  |  |  | `purpose`: `shipping_address` |
| `state1` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state_1` |
| `state1_taxid` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `state_1_tax_id` |
| `state2` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `state_2` |
| `state2_taxid` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `state_2_tax_id` |
| `state3` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `state_3` |
| `state3_taxid` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `state_3_tax_id` |
| `state4` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `state_4` |
| `state4_taxid` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `state_4_tax_id` |
| `state5` | VARCHAR2 | 2 |  |  |  |  | `purpose`: `state_5` |
| `state5_taxid` | VARCHAR2 | 20 |  |  |  |  | `purpose`: `state_5_tax_id` |
| `web_site` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `web_site` |
| `zip` | VARCHAR2 | 108 |  |  |  |  | `purpose`: `zip_code` |
