# Email Template (`emailtemplate`)
<!-- id: record:emailtemplate -->

| Fact | Value |
|---|---|
| Internal ID | `emailtemplate` |
| Category | marketing |
| Client scripts | no |
| Server scripts | yes |
| Searchable | no |
| Tags | `custom_fields_supported`, `no_search` |

## Record facts

- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `requires_feature`: `Customer Relationship Management` [`real_name`]
- `script_types`: `server`
- `user_events`: `beforeLoad`, `beforeSubmit`, `afterSubmit`
- `rule`: `only_one_of_content_or_mediaitem_has_value`
- `not_supported`: `scriptable_templates`
- `rule`: `only_records_browser_fields_supported`
- `purpose`: `crmsdk_and_scriptable_email_templates`
- `ui_location`: `documents_templates_email_templates`
- `configured_by`: `enable_features_crm_subtab` [`customer_relationship_management`]
- `displayed_on`: `template_subtab_text_editor` [`content`]
- `displayed_on`: `template_subtab_file_box` [`mediaitem`]
- `value_kind`: `plain_text_or_html` [`content`]

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `addcompanyaddress` | checkbox | Add Company Address to Message Footer in Bulk Merges | no | no |  | `purpose`: `company_address_in_footer` |
| `addunsubscribelink` | checkbox | Add Unsubscribe Link to Message Footer in Bulk Merges | no | no |  | `purpose`: `unsubscribe_link_in_footer`; `applies_to`: `bulk_email` |
| `campaigndomain` | select | Campaign Domain | no | no |  | `purpose`: `campaign_email_domain`; `effect`: `replaces_netsuite_domain_in_messages`; `required_when`: `over_10000_campaign_emails_per_month`; `allowed_values_listed_in`: `hosted_domains` |
| `content` | textarea |  | no | no |  | `purpose`: `template_content`; `caveat`: `selected_file_overrides_text` |
| `description` | textarea | Description | no | no |  | `purpose`: `template_description`; `not_displayed_on`: `sent_email` |
| `isinactive` | checkbox | Inactive | no | no |  | `purpose`: `inactivate_template`; `cannot`: `send_with_inactive_template` |
| `isprivate` | checkbox | Private | no | no |  | `purpose`: `private_template`; `true_means`: `only_owner_can_use` |
| `mediaitem` | select |  | no | no |  | `purpose`: `template_file` |
| `name` | text | Name | yes | no |  | `purpose`: `template_name`; `displayed_on`: `template_selection_list` |
| `restricttogroup` | select | Restrict To Group | no | no |  | `purpose`: `restrict_template_to_group` |
| `subject` | text | Subject | no | no |  | `purpose`: `default_email_subject`; `can`: `edit_per_message` |
| `usesmedia` | radio | File | no | no |  | `purpose`: `template_content_source`; `options`: `File`, `Text Editor` |
| `externalid` | text | ExternalId | no | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `marketing` | Marketing |
| `restriction` | Restrict Access |
| `template` | Template |
