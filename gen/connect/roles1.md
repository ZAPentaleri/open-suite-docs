# Roles1 (`roles1`)
<!-- id: connect:roles1 -->

| Fact | Value |
|---|---|
| Table | `roles1` |
| Record | `role` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `center_created_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `center_created_by` |
| `center_created_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `center_created_date` |
| `center_id` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `center_id` |
| `center_modified_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `center_modified_by` |
| `center_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `center_modified_date` |
| `center_name` | VARCHAR2 | 128 |  |  |  |  | `purpose`: `center_name` |
| `class_restriction_type` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `class_restriction_type` |
| `custom_record_permission_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `custom_record_permission_name` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `department_restriction_type` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `department_restriction_type` |
| `employee_restriction_type` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `employee_restriction_type` |
| `is_classallowviewing` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_class_allows_viewing` |
| `is_departmentallowviewing` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_department_allows_viewing` |
| `is_employeeallowviewing` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_employee_allows_viewing` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `role_is_inactive` |
| `is_itemclassrestricted` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `role_restricted_by_item_class` |
| `is_itemdepartmentrestricted` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `role_restricted_by_item_department` |
| `is_itemlocationtrestricted` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `role_restricted_by_item_location` |
| `is_locationallowviewing` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_location_allows_viewing` |
| `is_subsidiaryallowviewing` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `whether_subsidiary_allows_viewing` |
| `location_restriction_type` | VARCHAR2 | 16 |  |  |  |  | `purpose`: `location_restriction_type` |
| `permission_id` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `permission_id` |
| `permission_level_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `permission_level_id` |
| `permission_level_name` | VARCHAR2 | 64 |  |  |  |  | `purpose`: `permission_level_name` |
| `permission_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `permission_name` |
| `permsofroles_date_modified` | TIMESTAMP |  |  |  |  |  | `purpose`: `role_permissions_modified_date` |
| `permsofroles_modified_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `role_permissions_modified_by` |
| `role_created_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `role_created_by` |
| `role_created_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `role_created_date` |
| `role_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `role_id` |
| `role_modified_by` | NUMBER |  | 39 | 0 |  |  | `purpose`: `role_modified_by` |
| `role_name` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `role_name` |
