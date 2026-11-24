# Topic (`topic`)
<!-- id: record:topic -->

| Fact | Value |
|---|---|
| Internal ID | `topic` |
| Category | support |
| Client scripts | no |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `parenttopic`, `parenttopic_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `script_types`: `server`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `description` | textarea | Brief Description | no | no |  | `purpose`: `topic_description`; `displayed_on`: `topic_list`, `published_topic`; `ui_effect`: `text_enhance_button_may_appear` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_record`; `ui_effect`: `removed_from_lists_unless_show_inactives` |
| `longdescription` | textarea | Detailed Description | no | no |  | `purpose`: `solution_or_topic_text`; `accepts_forms`: `javascript`, `html`, `crmsdk_tags`; `ui_effect`: `preview_shows_published_view`; `ui_effect`: `text_enhance_button_may_appear` |
| `parenttopic` | select | Subtopic of | no | no |  | `purpose`: `parent_topic`; `default_behavior`: `top_level` (when `blank`) |
| `title` | text | Title | yes | no |  | `purpose`: `solution_title`; `supported_in`: `search`; `displayed_on`: `solutions_subtab`, `knowledge_bases` |
| `externalid` | text | ExternalId | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `notetab` | Notes |
| `publishtab` | Publish |
| `solutiontab` | Solutions |
| `subtopictab` | SubTopics |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Title |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `description` | text | Description |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Title |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `solution` | Solution | Solution |
