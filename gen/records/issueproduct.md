# Product (`issueproduct`)
<!-- id: record:issueproduct -->

| Fact | Value |
|---|---|
| Internal ID | `issueproduct` |
| Category | support |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | no |
| Tags | `custom_fields_supported`, `no_copy`, `no_search`, `no_transform` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `read`, `update`, `delete`
- `not_supported`: `copy`, `transform`, `search`
- `purpose`: `product_team_responsible_for_issue`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_record`; `doc_inconsistency`: `help_says_issue_role` |
| `issueproduct` | text | Product | yes |  |  | `purpose`: `parent_product`; `value_source`: `system_displayed` |
| `issueproductdescription` | textarea | Description | no |  |  | `purpose`: `product_description` |
| `showinactiveversions` | checkbox | Show Inactives | no |  |  | `purpose`: `show_inactive_versions`; `ui_effect`: `inactive_column_allows_reactivation` |
| `externalid` | text | ExternalId | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |

## Sublist `issuemodule` (Product Modules)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `build` | text | Module | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | integer | Internal ID | no |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |
| `issueproduct` | select | Product | no |  |  |

## Sublist `issueproductversions` (Version)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `allowbrokenin` | checkbox | Allow Broken In | no |  |  |
| `allowfixedin` | checkbox | Allow Fixed In | no |  |  |
| `allowtarget` | checkbox | Allow Target | no |  |  |
| `expectedreleasedate` | date | Expected Release Date | no |  |  |
| `externalid` | text | ExternalId | no |  |  |
| `externalreleasetarget` | text | External Target | no |  |  |
| `id` | integer | Internal ID | no |  |  |
| `internalid` | integer | Internal ID | no |  |  |
| `isefix` | checkbox | E-fix | no |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |
| `issueproduct` | select | Product | no |  |  |
| `issueversiondescription` | textarea | Description | no |  |  |
| `portletdescription` | text | Portlet Description | no |  |  |
| `productversion` | text | Product Version | yes |  |  |
| `showinportlet` | checkbox | Show In Portlet | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_says_issue_role` (on `isinactive`)
<!-- source-error: record:issueproduct | isinactive | help_says_issue_role -->
