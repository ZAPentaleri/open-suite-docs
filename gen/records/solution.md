# Solution (`solution`)
<!-- id: record:solution -->

| Fact | Value |
|---|---|
| Internal ID | `solution` |
| Category | support |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Knowledge Base` [`real_name`]
- `purpose`: `knowledge_base_solution`
- `configured_by`: `enable_features_crm_subtab` [`knowledge_base`]
- `ui_location`: `lists_support_solutions_new`
- `doc_inconsistency`: `ui_access_sentence_names_inventory_cost_revaluation`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `assigned` | select | Assigned To | no | yes |  | `purpose`: `assignee` |
| `autoname` | checkbox | Auto | no | no |  | `purpose`: `auto_code`; `shown_only_when`: `override_auto_numbers_allowed`; `false_means`: `code_editable` |
| `createddate` | datetime | Creation Date/Time | no | no |  | `purpose`: `date_created`; `value_source`: `system_displayed` |
| `customform` | select | Custom Form | no | no |  | `purpose`: `solution_form`; `ui_effect`: `page_switches_to_form`; `ui_effect`: `new_option_creates_record` |
| `displayonline` | checkbox | Display Online | no | yes |  | `purpose`: `display_in_knowledge_base`; `false_means`: `hidden_when_topic_published` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `ui_effect`: `removed_from_lists_unless_show_inactives` |
| `lastmodifieddate` | datetime | Last Modified Date/Time | no | no |  | `purpose`: `date_time_last_modified`; `value_source`: `system_displayed` |
| `longdescription` | textarea | Description | yes | no |  | `purpose`: `solution_or_topic_text`; `accepts_forms`: `javascript`, `html`, `crmsdk_tags`; `ui_effect`: `preview_shows_published_view`; `ui_effect`: `text_enhance_button_may_appear` |
| `message` | textarea | Abstract | yes | yes |  | `purpose`: `solution_abstract`; `format`: `plain_text_only`; `displayed_on`: `solutions_list`, `solutions_subtab`, `knowledge_bases`; `help_text_shared_with`: `solution.solutions.message` |
| `solutioncode` | text | Code | no | no |  | `purpose`: `solution_code`; `default_behavior`: `next_available_code`; `format`: `letters_and_numbers` |
| `status` | select | Status | yes | no |  | `purpose`: `solution_status`; `cannot`: `publish_unapproved` |
| `title` | text | Title | yes | yes |  | `purpose`: `solution_title`; `supported_in`: `search`; `displayed_on`: `solutions_subtab`, `knowledge_bases` |
| `eventnumber` | text |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `solutions` (Solutions)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `message` | textarea | Abstract | no |  | `purpose`: `solution_abstract`; `format`: `plain_text_only`; `displayed_on`: `solutions_list`, `solutions_subtab`, `knowledge_bases`; `help_text_shared_with`: `solution.message` |
| `id` | integer |  | no |  |  |
| `solution` | select | Code/Title | yes |  |  |

## Sublist `topics` (Topics)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `topic` | select | Topic | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `communication` | Communication |
| `custom` | Custom |
| `detail` | Details |
| `notes` | Notes |
| `solution` | Solutions |
| `support` | Cases |
| `topic` | Topics |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `abstract` | textarea | Abstract |  |
| `assigned` | select | Assigned To |  |
| `casecount` | integer | Case Count |  |
| `code` | text | Code |  |
| `createddate` | datetime | Date Created |  |
| `description` | textarea | Detail |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `find` | text | Keyword |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `isonline` | checkbox | Display Online |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `number` | integer | Number |  |
| `status` | select | Status |  |
| `title` | text | Title |  |
| `topic` | select | Topic |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `assigned` | select | Assigned To |  |
| `casecount` | integer | Case Count |  |
| `createddate` | datetime | Date Created |  |
| `description` | textarea | Detailed Description |  |
| `displayonline` | checkbox | Display Online |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `message` | textarea | Abstract |  |
| `previewref` | text | Preview |  |
| `solutioncode` | text | Code |  |
| `status` | select | Status |  |
| `title` | text | Title |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `relatedsolution` | Related Solution | Solution |
| `topic` | Topic | Topic |

## Suspected source errors

> [!SOURCE-ERROR] `ui_access_sentence_names_inventory_cost_revaluation`
<!-- source-error: record:solution | - | ui_access_sentence_names_inventory_cost_revaluation -->
