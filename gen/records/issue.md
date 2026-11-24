# Issue (`issue`)
<!-- id: record:issue -->

| Fact | Value |
|---|---|
| Internal ID | `issue` |
| Category | support |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `assigned` | select | Assigned To | no | yes |  | `purpose`: `assignee`; `caveat`: `list_depends_on_role_permissions` |
| `buildbroken` | select | Broken In Build | no | yes |  | `purpose`: `build_where_found` |
| `buildfixed` | select | Fixed In Build | no | yes |  | `purpose`: `build_where_fixed` |
| `buildtarget` | select | Target Build | no | yes |  | `purpose`: `target_fix_build` |
| `createddate` | datetime | Created | no | no |  | `purpose`: `date_created`; `value_source`: `system_displayed` |
| `emailassignee` | checkbox | Email Assignee | no | no |  | `purpose`: `email_assignee` |
| `emailcells` | select | Email Functional Teams | no | no |  | `purpose`: `email_to_cells`; `value_kind`: `multi_select` |
| `emailemployees` | select | Copy Employees | no | no |  | `purpose`: `email_to_employees`; `value_kind`: `multi_select`; `ui_effect`: `contributors_emails_everyone_involved` |
| `externalabstract` | textarea | External Abstract | no | no |  | `purpose`: `external_abstract`; `displayed_on`: `support_center`; `ui_effect`: `text_enhance_button_may_appear` |
| `externaldetails` | textarea | External Details | no | no |  | `purpose`: `external_details`; `displayed_on`: `support_center`; `ui_effect`: `text_enhance_button_may_appear` |
| `isreviewed` | checkbox | Reviewed | no | no |  | `purpose`: `resolution_reviewed` |
| `isshowstopper` | checkbox | Showstopper | no | yes |  | `purpose`: `showstopper`; `true_means`: `must_fix_before_next_release` |
| `issueabstract` | textarea | Abstract | yes | no |  | `purpose`: `internal_abstract`; `displayed_on`: `support_center` (when `no_external_abstract`); `ui_effect`: `text_enhance_button_may_appear` |
| `issuenumber` | identifieranycase | Issue Number | yes | no |  | `purpose`: `issue_number`; `value_source`: `auto_generated` (when `auto_generated_numbering`) |
| `issuestatus` | select | Issue Status | yes | yes |  | `purpose`: `issue_status`; `recommendation`: `update_on_each_change_or_reassign` |
| `issuetags` | select | Tags | no | no |  | `purpose`: `issue_tags`; `value_kind`: `multi_select`; `effect`: `improves_searchability` |
| `issuetype` | select | Issue Type | yes | no |  | `purpose`: `issue_type`; `allowed_values_listed_in`: `issue_type_records` |
| `item` | select | Item | no | no |  | `purpose`: `related_item` |
| `module` | select | Module | no | no |  | `purpose`: `product_module` |
| `newdetails` | textarea | New Details | no | no |  | `purpose`: `new_details`; `ui_effect`: `text_enhance_button_may_appear` |
| `priority` | select | Priority | yes | yes |  | `purpose`: `priority`; `allowed_range`: `1`, `9`; `option_meanings`: `1:most_urgent`, `9:least_urgent` |
| `product` | select | Product | yes | no |  | `purpose`: `product`; `allowed_values_listed_in`: `issue_product_records`; `caveat`: `products_separate_from_items` |
| `productteam` | select | Product Team | no | yes |  | `purpose`: `product_team`; `effect`: `team_emailed_on_new_issue` |
| `reportedby` | select | Reported By | no | no |  | `purpose`: `reported_by`; `value_source`: `record_creator`; `read_only_when`: `user_interface`; `can`: `set_in_suitescript_on_create`; `read_only_when`: `existing_record` [`suitescript_and_web_services`] |
| `reproduce` | select | Reproduced | no | no |  | `purpose`: `reproduction_environment`; `options`: `In test environment`, `In user's environment ONLY`, `Not reproduced` |
| `reviewer` | select | Reviewer | no | no |  | `purpose`: `reviewer` |
| `severity` | select | Severity | yes | yes |  | `purpose`: `severity`; `allowed_values_listed_in`: `issue_severity_list` |
| `source` | select | Source | no | no |  | `purpose`: `issue_origin` |
| `versionbroken` | select | Broken In Version | no | yes |  | `purpose`: `version_where_found`; `allowed_values_listed_in`: `product_versions_on_product_record` |
| `versionfixed` | select | Fixed In Version | no | yes |  | `purpose`: `version_where_resolved` |
| `versiontarget` | select | Target Version | no | yes |  | `purpose`: `target_fix_version`; `allowed_values_listed_in`: `product_versions_on_product_record` |
| `autoname` | checkbox | Auto | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `datereleased` | text | Date Released | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime | Modification Date | no | no |  |  |

## Sublist `brokeninversion` (Broken In)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `build` | select | Build | no |  |  |
| `primary` | checkbox | Primary | no |  |  |
| `version` | select | Version | yes |  |  |

## Sublist `fixedinversion` (Fixed In)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `build` | select | Build | no |  |  |
| `primary` | checkbox | Primary | no |  |  |
| `version` | select | Version | yes |  |  |

## Sublist `relatedissues` (Issues)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `issueabstract` | url | Abstract | no |  |  |
| `issueassignee` | text | Assigned To | no |  |  |
| `issuenumber` | select | Issue No. | yes |  |  |
| `issuestatus` | text | Status | no |  |  |
| `relationship` | select | Relationship | yes |  |  |
| `relationshipcomment` | textarea | Comment | no |  |  |

## Sublist `targetversion` (Target)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `build` | select | Build | no |  |  |
| `primary` | checkbox | Primary | no |  |  |
| `version` | select | Version | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `case` | Cases |
| `communication` | Communication |
| `custom` | Custom |
| `detail` | Details |
| `external` | External |
| `file` | Files |
| `version` | Versions |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `ageinmonths` | integer | Age in Months |  |
| `assigned` | select | Assigned To |  |
| `buildbroken` | select | Broken In Build |  |
| `buildbrokenname` | text | Broken In Build (Name) |  |
| `buildfixed` | select | Fixed In Build |  |
| `buildfixedname` | text | Fixed In Build (Name) |  |
| `buildtarget` | select | Target Build |  |
| `buildtargetname` | text | Target Build (Name) |  |
| `casecount` | integer | Case Count |  |
| `casenumber` | text | Case Number |  |
| `closeddate` | datetime | Date Closed |  |
| `createddate` | datetime | Date Created |  |
| `datereleased` | date | Date Released |  |
| `details` | text | Details |  |
| `duplicateof` | select | Duplicate Of |  |
| `efix` | checkbox | E-Fix |  |
| `employeeorteam` | select | Employee/Team |  |
| `eventstatus` | select | Base Status |  |
| `externalabstract` | text | External Abstract |  |
| `externaldetails` | text | External Details |  |
| `externalfixedin` | select | Fixed In Version (External) |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `externalstatus` | select | External Status |  |
| `fixed` | datetime | Date Fixed |  |
| `fixedby` | select | Fixed By |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isowner` | checkbox | I Own This Issue |  |
| `isreviewed` | checkbox | Reviewed |  |
| `isshowstopper` | checkbox | Showstopper |  |
| `issueabstract` | text | Abstract |  |
| `issuenumber` | text | Number/ID |  |
| `item` | select | Item |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `module` | select | Module |  |
| `number` | integer | Number |  |
| `originalfixedin` | text | Last Version Fixed (pre-11) |  |
| `priority` | select | Priority |  |
| `product` | select | Product |  |
| `productteam` | select | Product Team |  |
| `reportedby` | select | Reported By |  |
| `reproduce` | select | Reproduced |  |
| `resolved` | datetime | Date Resolved |  |
| `resolvedby` | select | Resolved By |  |
| `reviewer` | select | Reviewer |  |
| `severity` | select | Severity |  |
| `source` | select | Source |  |
| `status` | select | Status |  |
| `tags` | multiselect | Tags |  |
| `tracking` | checkbox | Tracking |  |
| `type` | select | Type |  |
| `usertype` | select | User Type |  |
| `versionbroken` | select | Broken In Version |  |
| `versionfixed` | select | Fixed In Version |  |
| `versiontarget` | select | Target Version |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `ageinmonths` | integer | Age in Months |  |
| `assigned` | select | Assigned To |  |
| `buildbroken` | select | Broken In Build |  |
| `buildfixed` | select | Fixed In Build |  |
| `buildtarget` | select | Target Build |  |
| `casecount` | integer | Case Count |  |
| `casenumber` | text | Case Number |  |
| `closeddate` | datetime | Date Closed |  |
| `createddate` | datetime | Date Created |  |
| `datereleased` | date | Date Released |  |
| `duplicateof` | select | Duplicate Of |  |
| `employeeorteam` | select | Employee/Team |  |
| `eventstatus` | select | Base Status |  |
| `externalabstract` | text | External Abstract |  |
| `externaldetails` | text | External Details |  |
| `externalfixedin` | text | Fixed In Version (External) |  |
| `externalid` | select | External ID |  |
| `externalstatus` | select | External Status |  |
| `fixed` | datetime | Date Fixed |  |
| `fixedby` | select | Fixed By |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isowner` | checkbox | I Own This Issue |  |
| `isreviewed` | checkbox | Reviewed |  |
| `isshowstopper` | select | Showstopper |  |
| `issueabstract` | text | Abstract |  |
| `issuestatus` | select | Status |  |
| `item` | select | Item |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `module` | text | Module |  |
| `number` | text | Number |  |
| `originalfixedin` | text | Last Version Fixed (pre-11) |  |
| `priority` | select | Priority |  |
| `product` | select | Product |  |
| `productteam` | select | Product Team |  |
| `reportedby` | select | Reported By |  |
| `reproduce` | select | Reproduced |  |
| `resolved` | datetime | Date Resolved |  |
| `resolvedby` | select | Resolved By |  |
| `reviewer` | select | Reviewed By |  |
| `severity` | select | Severity |  |
| `source` | select | Source |  |
| `tags` | multiselect | Tags |  |
| `type` | text | Type |  |
| `usertype` | select | User Type |  |
| `versionbroken` | select | Broken In Version |  |
| `versionfixed` | select | Fixed In Version |  |
| `versiontarget` | select | Target Version |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `employee` | Employee | Employee |
