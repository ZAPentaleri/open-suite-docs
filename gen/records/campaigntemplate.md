# Campaign Template (`campaigntemplate`)
<!-- id: record:campaigntemplate -->

| Fact | Value |
|---|---|
| Internal ID | `campaigntemplate` |
| Category | marketing |
| Client scripts | no |
| Server scripts | yes |
| Searchable | no |
| Tags | `custom_fields_supported`, `no_search` |

## Record facts

- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `rule`: `only_one_of_content_or_mediaitem_has_value`
- `not_supported`: `scriptable_templates`
- `rule`: `only_records_browser_fields_supported`
- `purpose`: `crmsdk_and_scriptable_campaign_templates`
- `ui_location`: `documents_templates_marketing_templates`
- `displayed_on`: `template_subtab_text_editor` [`content`]
- `displayed_on`: `template_subtab_file_box` [`mediaitem`]
- `value_kind`: `plain_text_or_html` [`content`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `campaigndomain` | select | Campaign Domain | no | no |  | `purpose`: `campaign_email_domain`; `effect`: `replaces_netsuite_domain_in_messages`; `required_when`: `over_10000_campaign_emails_per_month`; `allowed_values_listed_in`: `hosted_domains` |
| `description` | textarea | Description | no | no |  | `purpose`: `template_description` |
| `emailassalesrep` | checkbox | Email as Sales Rep | no | no |  | `purpose`: `email_as_sales_rep`; `effect`: `replies_go_to_sales_rep`; `value_source`: `rep_nickname_and_from_address`; `default_behavior`: `template_from_values` (when `rep_has_no_profile`); `effect`: `overrides_reply_to_address`; `value_source`: `rep_user_profile_preferences` |
| `fromemail` | select | From Email Address | yes | no |  | `purpose`: `from_email_address`; `requires`: `address_authorized_for_marketing`; `allowed_values_listed_in`: `authorized_marketing_email_addresses` |
| `fromname` | text | From Name | no | no |  | `purpose`: `from_name` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_template`; `cannot`: `use_inactive_template_in_campaigns` |
| `isprivate` | checkbox | Private | no | no |  | `purpose`: `private_template`; `true_means`: `only_owner_can_use` |
| `name` | text | Name | yes | no |  | `purpose`: `template_name` |
| `replytoemail` | select | Reply to Email Address | yes | no |  | `purpose`: `reply_to_email_address`; `requires`: `address_authorized_for_marketing`; `allowed_values_listed_in`: `authorized_marketing_email_addresses` |
| `restricttogroup` | select | Restrict To Group | no | no |  | `purpose`: `restrict_template_to_group` |
| `subject` | text | Subject | no | no |  | `purpose`: `template_subject`; `accepts_forms`: `crmsdk_tags`; `displayed_on`: `email_subject`, `online_form_browser_title` |
| `trackstatistics` | checkbox | Track Outgoing Email | no | no |  | `purpose`: `track_email_statistics`; `caveat`: `only_netsuite_links_tracked`; `displayed_on`: `campaign_statistics_reports`; `effect`: `open_or_click_sends_request_to_netsuite`; `recommendation`: `address_tracking_in_privacy_policy`; `value_examples`: `site_and_online_form_links_tracked_third_party_not` |
| `usesmedia` | radio | File | no | no |  | `purpose`: `template_content_source`; `options`: `File`, `Text Editor` |
| `content` | textarea |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `mediaitem` | select |  | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `marketing` | Marketing |
| `restriction` | Restrict Access |
| `template` | Template |
