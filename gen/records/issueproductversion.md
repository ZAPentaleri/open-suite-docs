# Version (`issueproductversion`)
<!-- id: record:issueproductversion -->

| Fact | Value |
|---|---|
| Internal ID | `issueproductversion` |
| Subrecord | yes |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `allowbrokenin` | checkbox | Allow Broken In | no |  |  | `purpose`: `version_selectable_as_broken_in` |
| `allowfixedin` | checkbox | Allow Fixed In | no |  |  | `purpose`: `version_selectable_as_fixed_in` |
| `allowtarget` | checkbox | Allow Target | no |  |  | `purpose`: `version_selectable_as_target` |
| `externalreleasetarget` | text | External Target | no |  |  | `purpose`: `customer_facing_version_name`; `displayed_on`: `customer_center_issue_records` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_record`; `doc_inconsistency`: `help_says_issue_role` |
| `issueproduct` | select | Product | no |  |  | `purpose`: `parent_product`; `value_source`: `system_displayed` |
| `expectedreleasedate` | date | Expected Release Date | no |  |  |  |
| `externalid` | text | ExternalId | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `isefix` | checkbox | E-fix | no |  |  |  |
| `issueversiondescription` | textarea | Description | no |  |  |  |
| `portletdescription` | text | Portlet Description | no |  |  |  |
| `productversion` | text | Product Version | yes |  |  |  |
| `showinportlet` | checkbox | Show In Portlet | no |  |  |  |

## Sublist `availablebuild` (Build)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `allowbrokenin` | checkbox | Allow Broken In | no |  |  |
| `allowfixedin` | checkbox | Allow Fixed In | no |  |  |
| `allowtarget` | checkbox | Allow Target | no |  |  |
| `build` | text | Build | no |  |  |
| `builtdate` | date | Build Date | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | integer | Internal ID | no |  |  |
| `isinactive` | checkbox | Inactive | no |  |  |
| `productversion` | select | Product Version | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `help_says_issue_role` (on `isinactive`)
<!-- source-error: record:issueproductversion | isinactive | help_says_issue_role -->
