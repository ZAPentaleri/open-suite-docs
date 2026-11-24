# Entity Group (`entitygroup`)
<!-- id: record:entitygroup -->

| Fact | Value |
|---|---|
| Internal ID | `entitygroup` |
| Category | list |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`, `search`
- `requires_feature`: `Marketing Automation` [`real_name`]
- `subrecords`: `groupOwner`, `groupType`, `issueRole`, `restrictionGroup`, `savedSearch`, `subsidiary`, `workCalendar`
- `not_scriptable`: `notes_files_events_workflows_tab_fields`, `bulkmerge_sublist`, `campaign_sublist`
- `rule`: `fields_vary_by_group_type`
- `requires_feature`: `Mail Merge`, `Team Selling`, `Customer Support and Service`, `Sales Force Automation`, `Issue Management`, `Manufacturing Routing and Work Center`, `Billing Operations`, `Subsidiaries`, `Personalized Catalog Views` (when `using_all_fields`) [`real_name`]
- `workaround`: `check_supported_fields_per_group_type_in_ui`
- `sublists`: `testcell`
- `script_types`: `server`
- `user_events_supported`: `false`
- `required_fields`: `grouptype:static_or_dynamic`
- `rule`: `dynamic_group_members_from_saved_search`
- `rule`: `static_members_only_set_at_create`
- `purpose`: `group_of_contacts_customers_employees_partners_or_vendors`
- `ui_location`: `lists_relationships_group_new`
- `difference`: `ui_allows_static_member_edits_after_create_script_does_not`
- `workaround`: `add_static_members_after_create_via_separate_code`
- `doc_inconsistency`: `listed_twice_conflicting_category_client_support_level` [`needs_human_review`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `billinggroup` | checkbox | Billing Group | no |  | marked_new | `purpose`: `billing_group`; `use_when`: `bill_runs` |
| `comments` | textarea | Comments | no | no |  | `purpose`: `group_comments`; `ui_effect`: `clean_up_button_polishes_text` |
| `email` | email | Email | no | no |  | `purpose`: `group_email_address`; `ui_effect`: `address_opens_mail_program` |
| `groupname` | text | Name | yes | no |  | `purpose`: `group_name` |
| `groupowner` | select | Owner | yes | no |  | `purpose`: `group_owner`; `default_behavior`: `current_user` |
| `grouptypename` | text | Type | no | no |  | `purpose`: `group_member_record_type`; `value_source`: `system_displayed` |
| `isfunctionalteam` | checkbox | Functional Team | no | no |  | `purpose`: `functional_team`; `effect`: `can_be_emailed_issue_copies` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_group`; `effect`: `no_shared_calendar_or_invitations`; `ui_effect`: `hidden_from_list_unless_show_inactives` |
| `ismanufacturingworkcenter` | checkbox | Manufacturing Work Center | no | no |  | `purpose`: `manufacturing_work_center`; `effect`: `usable_with_routing_records` |
| `isprivate` | checkbox | Private | no | no |  | `purpose`: `private_member_list` |
| `isproductteam` | checkbox | Product Team | no | no |  | `purpose`: `product_team`; `effect`: `members_emailed_on_new_issue` |
| `issalesrep` | checkbox | Sales Group | no | no |  | `purpose`: `sales_group`; `effect`: `listed_in_sales_rep_fields` |
| `issuerole` | select | Issue Role | no | no |  | `purpose`: `issue_role`; `effect`: `group_assignable_to_issues` |
| `issupportrep` | checkbox | Support Group | no | no |  | `purpose`: `support_group`; `effect`: `listed_in_case_assignee_fields` |
| `laborresources` | double | Labor Resources | no | no |  | `purpose`: `labor_resource_count` |
| `machineresources` | double | Machine Resources | no | no |  | `purpose`: `machine_resource_count` |
| `restrictiongroup` | select | Restrict To | no | no |  | `purpose`: `restrict_group_use`; `constraint`: `restricted_group_cannot_be_private` |
| `savedsearch` | select | Saved Search | yes | no |  | `purpose`: `member_saved_search`; `applies_when`: `dynamic_group`; `allowed_values_listed_in`: `saved_search_records` |
| `workcalendar` | select | Work Calendar | no | no |  | `purpose`: `work_center_calendar` |
| `externalid` | text | ExternalId | no | no |  |  |
| `grouptype` | select | Group Type | no |  | marked_new |  |
| `id` | integer | Internal ID | no |  | marked_new |  |
| `subsidiary` | select | Subsidiary | yes | no |  |  |

## Sublist `groupmembers` (Group Members)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accesslevel` | select | Access Level | no |  |  |
| `bouncedaddress` | text | Bounced | no |  |  |
| `company` | select | Company | no | marked_new |  |
| `contactmember` | select | Contact | yes | marked_new |  |
| `contribution` | percent | Contribution % | yes |  |  |
| `custjobmember` | select | Customer | yes | marked_new |  |
| `email` | email | Email | no |  |  |
| `employeemember` | select | Name | yes |  |  |
| `inactive` | text | Inactive | no |  |  |
| `isinactive` | checkbox | Inactive | no | marked_new |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `issalesrep` | text |  | no |  |  |
| `jobmember` | select | Project | yes | marked_new |  |
| `member` | select | Entity | yes | marked_new |  |
| `memberemail` | text | Email | no |  |  |
| `membername` | text | Name | no |  |  |
| `memberphone` | phone | Phone | no |  |  |
| `name` | text | Name | no | marked_new |  |
| `partnermember` | select | Partner | yes | marked_new |  |
| `phone` | phone | Phone | no |  |  |
| `remove` | text | Remove | no |  |  |
| `salesrep` | select | Sales Rep | no | marked_new |  |
| `salesrole` | select | Sales Role | yes |  |  |
| `subscriptionstatus` | text | Global Subscription Status | no |  |  |
| `vendormember` | select | Vendor | yes | marked_new |  |

## Sublist `testcell` (Test Cell)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `cellid` | integer | ID | no | marked_new |  |
| `cellname` | text | Name | yes | marked_new |  |
| `cellpercent` | percent | Percentage | yes | marked_new |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `email` | text | Email |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `groupname` | text | Name |  |
| `groupowner` | select | Owner |  |
| `grouptype` | select | Type |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdynamic` | checkbox | Dynamic |  |
| `isinactive` | checkbox | Inactive |  |
| `ismanufacturingworkcenter` | checkbox | Manufacturing Work Center |  |
| `isprivate` | checkbox | Private |  |
| `laborresources` | double | Labor Resources |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `machineresources` | double | Machine Resources |  |
| `subsidiary` | select | Subsidiary |  |
| `workcalendar` | select | Work Calendar |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `email` | email | Email |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `groupname` | text | Name |  |
| `grouptype` | text | Type |  |
| `internalid` | select | Internal ID |  |
| `isdynamic` | checkbox | Dynamic |  |
| `isinactive` | checkbox | Inactive |  |
| `ismanufacturingworkcenter` | checkbox | Manufacturing Work Center |  |
| `isprivate` | checkbox | Private |  |
| `laborresources` | double | Labor Resources |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `machineresources` | double | Machine Resources |  |
| `owner` | select | Owner |  |
| `savedsearch` | text | Saved Search |  |
| `size` | integer | Size |  |
| `subsidiary` | select | Subsidiary |  |
| `workcalendar` | select | Work Calendar |  |

## Suspected source errors

> [!SOURCE-ERROR] `listed_twice_conflicting_category_client_support_level`
<!-- source-error: record:entitygroup | - | listed_twice_conflicting_category_client_support_level -->
