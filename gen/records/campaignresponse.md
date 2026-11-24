# Campaign Response (`campaignresponse`)
<!-- id: record:campaignresponse -->

| Fact | Value |
|---|---|
| Internal ID | `campaignresponse` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `not_supported`: `copy`, `search`, `transform`
- `script_functions`: `create`, `read`, `update`, `delete`
- `requires_feature`: `Marketing Automation` [`real_name`]
- `read_only_sublists`: `responses`
- `rule`: `responses_sublist_appended_on_each_save`
- `purpose`: `refine_marketing_campaign_delivery`
- `ui_location`: `entity_marketing_subtab_campaigns_add_response`
- `value_kind`: `response_entry_author_date_note_response` [`responses_sublist`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `campaignevent` | select | Event | no | no |  | `purpose`: `campaign_event` |
| `campaignresponsedate` | date | Date Created | no | no |  | `purpose`: `campaign_sent_date` |
| `channel` | text | Channel | no | no |  | `purpose`: `campaign_channel` |
| `entity` | select | Recipient | yes | no |  | `purpose`: `recipient` |
| `leadsource` | select | Campaign | no | no |  | `purpose`: `campaign` |
| `note` | textarea | Note | no | no |  | `purpose`: `response_note`; `help_text_shared_with`: `campaignresponse.responses.note` |
| `response` | select | New Response Detail | no | no |  | `purpose`: `recipient_response`; `effect`: `previous_responses_kept_in_responses_sublist` |
| `externalid` | text | ExternalId | no | no |  |  |

## Sublist `responses` (Response History)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `note` | text | Notes | no |  | `purpose`: `response_note`; `help_text_shared_with`: `campaignresponse.note` |
| `response` | text | Response Detail | no |  | `purpose`: `recipient_response` |
| `author` | text | Author | no |  |  |
| `responsedate` | datetime | Date/Time | no |  |  |
