# Role (`role`)
<!-- id: record:role -->

| Fact | Value |
|---|---|
| Internal ID | `role` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `supports_search` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `search`
- `requires_permission`: `manage_roles`
- `client_deployable`: `false`
- `client_via`: `browser_console_only`
- `rule`: `search_type_role_case_sensitive` (refs `N/search.Type`)
- `not_supported`: `duplicate_search_apis`
- `purpose`: `access_configuration_assigned_to_users`
- `ui_location`: `setup_users_roles_manage_roles_search`
- `format`: `capitalized_role_type_id` (when `passing_role_to_search`)

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `centertype` | select | Center Type |  |
| `classallowview` | checkbox | Class Allow Viewing |  |
| `classrestriction` | select | Class Restrictions |  |
| `departmentrestriction` | select | Department Restrictions |  |
| `deptallowview` | checkbox | Department Allow Viewing |  |
| `empallowview` | checkbox | Employee Allow Viewing |  |
| `employeerestriction` | select | Employee Restrictions |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `iscustom` | checkbox | Custom |  |
| `isinactive` | checkbox | Inactive |  |
| `ispartnerrole` | checkbox | Is Partner Role |  |
| `issalesrole` | checkbox | Is Sales Role |  |
| `isssoonly` | checkbox | Is Single Sign On Only |  |
| `issupportrole` | checkbox | Is Support Role |  |
| `iswsonly` | checkbox | Is Web Services Only |  |
| `level` | select | Level |  |
| `locallowview` | checkbox | Location Allow Viewing |  |
| `locationrestriction` | select | Location Restrictions |  |
| `name` | text | Name |  |
| `permchangedate` | datetime | Permission Change Date |  |
| `permission` | select | Permission |  |
| `permissionchange` | select | Permission Change |  |
| `restrictitembyclass` | checkbox | Restrict Item By Class |  |
| `restrictitembydepartment` | checkbox | Restrict Item By Department |  |
| `restrictitembylocation` | checkbox | Restrict Item By Location |  |
| `subsidallowview` | checkbox | Subsidiary Allow Viewing |  |
| `subsidiaries` | select | Subsidiaries |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `centertype` | select | Center Type |  |
| `classallowview` | checkbox | Class Allow Viewing |  |
| `classrestriction` | select | Class Restrictions |  |
| `customstandard` | text | Custom/Standard |  |
| `departmentrestriction` | select | Department Restrictions |  |
| `deptallowview` | checkbox | Department Allow Viewing |  |
| `empallowview` | checkbox | Employee Allow Viewing |  |
| `employeerestriction` | select | Employee Restrictions |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `frombundle` | text | From Bundle |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `ispartnerrole` | checkbox | Is Partner Role |  |
| `issalesrole` | checkbox | Is Sales Role |  |
| `isssoonly` | checkbox | Is Single Sign On Only |  |
| `issupportrole` | checkbox | Is Support Role |  |
| `iswsonly` | checkbox | Is Web Services Only |  |
| `level` | select | Level |  |
| `locallowview` | checkbox | Location Allow Viewing |  |
| `locationrestriction` | select | Location Restrictions |  |
| `name` | text | Name |  |
| `permchangedate` | datetime | Permission Change Date |  |
| `permchangelevel` | text | Permission Change Level |  |
| `permission` | select | Permission |  |
| `permissionchange` | text | Permission Change |  |
| `restrictitembyclass` | checkbox | Restrict Item By Class |  |
| `restrictitembydepartment` | checkbox | Restrict Item By Department |  |
| `restrictitembylocation` | checkbox | Restrict Item By Location |  |
| `subsidallowview` | checkbox | Subsidiary Allow Viewing |  |
| `subsidiaries` | select | Subsidiaries |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `user` | User | Employee |
