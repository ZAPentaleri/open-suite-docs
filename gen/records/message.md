# Message (`message`)
<!-- id: record:message -->

| Fact | Value |
|---|---|
| Internal ID | `message` |
| Category | communication |
| Client scripts | no |
| Server scripts | yes |
| Tags | `custom_fields_supported` |

## Record facts

- `script_types`: `server`
- `related` (refs `N/email`)
- `editable_only_during`: `create`
- `script_functions`: `create`, `copy`, `delete`
- `user_events_when_inbound_case_capture`: `beforeLoad`, `afterSubmit`
- `cc_bcc_via_sublist`: `otherrecipientslist`
- `required_when`: `authoremail:author_entity_has_no_email`
- `required_when`: `recipientemail:recipient_entity_has_no_email`
- `not_supported`: `other_message_types`
- `purpose`: `record_correspondence_with_business`
- `effect`: `related_emails_auto_attached_to_record_and_recipients`
- `applies_to`: `customer`, `contact`, `opportunity`
- `recommendation`: `do_not_delete_messages_breaks_threads`
- `requires`: `delete_permission` (when `deleting_message`)
- `not_supported`: `before_submit_user_event` (when `created_by_inbound_email_case_capture`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `author` | select | From | no | no |  | `purpose`: `message_author` |
| `authoremail` | email | From Email Address | no | no |  | `purpose`: `sender_email` |
| `bcc` | emails | Bcc | no | no |  | `purpose`: `bcc_addresses`; `format`: `comma_or_semicolon_separated`; `not_displayed_on`: `other_recipients` |
| `cc` | emails | Cc | no | no |  | `purpose`: `cc_addresses`; `format`: `comma_or_semicolon_separated`; `displayed_on`: `all_recipients` |
| `emailed` | checkbox | Emailed | no | no |  | `purpose`: `pasted_email_message` |
| `message` | textarea | Message | no | no |  | `purpose`: `message_text` |
| `recipient` | select | Attach to | no | no |  | `purpose`: `message_recipient`; `effect`: `saved_on_recipient_messages_subtab` |
| `recipientemail` | email | To (Email) | no | no |  | `purpose`: `recipient_email` |
| `subject` | text | Subject | yes | no |  | `purpose`: `message_subject`; `displayed_on`: `messages_subtab` |
| `template` | select | Template | no | no |  | `purpose`: `email_template`; `can`: `edit_message_after_template`; `ui_effect`: `new_option_creates_record` |
| `activity` | text |  | no | no |  |  |
| `contact` | text |  | no | no |  |  |
| `entity` | text |  | no | no |  |  |
| `entitytype` | text |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `hasattachment` | text |  | no | no |  |  |
| `lastmodifieddate` | datetime | Modification Date | no | no |  |  |
| `messagedate` | date | Date | no | no |  |  |
| `record` | text |  | no | no |  |  |
| `recordtype` | text |  | no | no |  |  |
| `time` | timeofday | Time | no | no |  |  |
| `transaction` | text |  | no | no |  |  |

## Sublist `mediaitem` (Attachments)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `mediaitem` | select | Attach File | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `attachments` | Attachments |
| `messages` | Message |
| `recipients` | Recipients |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `author` | select | Author |  |
| `authoremail` | email | From Email |  |
| `bcc` | email | Bcc |  |
| `cc` | email | Cc |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `hasattachment` | checkbox | Has Attachment |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `internalonly` | checkbox | Internal Only |  |
| `isincoming` | checkbox | Is Incoming |  |
| `message` | clobtext | Message |  |
| `messagedate` | datetime | Date |  |
| `messagetype` | select | Type |  |
| `recipient` | select | Recipient |  |
| `recipientemail` | email | Recipient Email |  |
| `recipients` | select | Recipients |  |
| `subject` | text | Subject |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `author` | select | Author |  |
| `authoremail` | text | Author Email |  |
| `bcc` | email | Bcc |  |
| `cc` | email | Cc |  |
| `externalid` | select | External ID |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `hasattachment` | checkbox | Files |  |
| `internalid` | select | Internal ID |  |
| `internalonly` | checkbox | Internal Only |  |
| `isemailed` | checkbox | Email Sent |  |
| `isincoming` | checkbox | Is Incoming |  |
| `message` | clobtext | Message |  |
| `messagedate` | datetime | Date |  |
| `messagetype` | select | Type |  |
| `recipient` | select | Recipient |  |
| `recipientemail` | text | Recipient Email |  |
| `recipients` | select | Recipients |  |
| `subject` | text | Subject |  |
| `view` | text | View |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `campaign` | Campaign | Campaign |
| `contact` | Contact | Contact |
| `customer` | Customer | Customer |
| `employee` | Employee | Employee |
| `originatinglead` | Originating Lead | Lead |
| `partner` | Partner | Partner |
| `vendor` | Vendor | Vendor |
