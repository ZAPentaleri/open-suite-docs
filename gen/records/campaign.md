# Campaign (`campaign`)
<!-- id: record:campaign -->

| Fact | Value |
|---|---|
| Internal ID | `campaign` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `audience` | select | Audience Description | no | yes |  |  | `purpose`: `campaign_audience`; `allowed_values_listed_in`: `campaign_audience_records`; `use_when`: `media_marketing_without_recipient_list` |
| `autoname` | checkbox | Auto | no | no |  |  | `purpose`: `auto_name_campaign`; `false_means`: `enter_campaign_id_manually` |
| `basecost` | currency | Base Cost | no | no |  |  | `purpose`: `campaign_base_cost`; `effect`: `added_to_event_costs_for_total` |
| `campaignid` | identifieranycase | ID | no | no |  |  | `purpose`: `campaign_id`; `value_source`: `auto_generated_on_save` (when `auto_numbering`) |
| `category` | select | Category | no | yes |  |  | `purpose`: `campaign_category`; `allowed_values_listed_in`: `campaign_category_records`; `effect`: `groups_campaigns_on_reports_and_lists` |
| `convcostpercustomer` | currency | Cost per Purchaser | no | no | yes |  | `purpose`: `cost_per_conversion`; `value_source`: `cost_divided_by_promo_code_transactions` |
| `conversions` | integer | Number of Purchasers | no | no |  |  | `purpose`: `conversions_count` |
| `cost` | currency | Total Cost | no | no | yes |  | `purpose`: `total_campaign_cost`; `value_source`: `base_cost_plus_event_costs`; `effect`: `used_in_roi`; `help_text_shared_with`: `campaign.campaigndirectmail.cost`, `campaign.campaignemail.cost`, `campaign.campaignevent.cost` |
| `costpercustomer` | currency | Cost per Lead | no | no | yes |  | `purpose`: `cost_per_new_customer`; `value_source`: `cost_divided_by_new_customers` |
| `customform` | select | Custom Form | no | no |  |  | `purpose`: `campaign_form`; `allowed_values_listed_in`: `custom_entry_forms`; `ui_effect`: `new_option_creates_record` |
| `enddate` | date | End Date | no | no |  |  | `purpose`: `campaign_end_date` |
| `expectedrevenue` | currency | Expected Revenue | no | yes |  |  | `purpose`: `expected_revenue` |
| `family` | select | Family | no | yes |  |  | `purpose`: `keyword_family`; `allowed_values_listed_in`: `campaign_family_records`; `value_kind`: `grouping_of_paid_keywords` |
| `isinactive` | checkbox | Inactive | no | no |  |  | `purpose`: `inactivate_record` |
| `item` | select | Item | no | no |  |  | `purpose`: `items_offered`; `value_kind`: `multi_select`; `supported_in`: `search` |
| `keyword` | text | Keyword | no | yes |  |  | `purpose`: `paid_keyword` |
| `leadsgenerated` | integer | Leads Generated | no | no |  |  | `purpose`: `leads_generated_count` |
| `message` | textarea | Description | no | yes |  |  | `purpose`: `campaign_description`; `displayed_on`: `campaign_record_only` |
| `offer` | select | Offer | no | yes |  |  | `purpose`: `campaign_offer`; `allowed_values_listed_in`: `campaign_offer_records` |
| `owner` | select | Manager | yes | yes |  |  | `purpose`: `campaign_manager`; `effect`: `notified_on_successful_event_delivery` |
| `profit` | currency | Profit | no | no | yes |  | `purpose`: `campaign_profit`; `value_source`: `revenue_minus_cost` |
| `promotioncode` | select | Promotion Code | no | no |  |  | `purpose`: `promotion_code`; `allowed_values_listed_in`: `promotion_code_records`; `effect`: `offers_discounts_via_campaign`; `requires`: `recipients_enter_code_in_web_store` |
| `roi` | percent | Return on Investment | no | no | yes |  | `purpose`: `campaign_roi`; `value_source`: `revenue_minus_cost_divided_by_cost` |
| `searchengine` | select | Search Engine | no | yes |  |  | `purpose`: `keyword_search_engine`; `allowed_values_listed_in`: `campaign_search_engine_records` |
| `startdate` | date | Start Date | yes | no |  |  | `purpose`: `campaign_start_date` |
| `title` | text | Title | yes | yes |  |  | `purpose`: `campaign_title`; `max_length`: `99` characters |
| `totalrevenue` | currency | Total Revenue | no | no |  |  | `purpose`: `total_revenue_of_campaign`; `value_source`: `system_displayed` |
| `uniquevisitors` | integer | Unique Visitors | no | no |  |  | `purpose`: `unique_visitors_count` |
| `url` | url | URL | no | yes |  |  | `purpose`: `landing_page_url`; `effect`: `leadsource_parameter_appended` (when `campaign_id_or_autonumber`) |
| `vertical` | select | Vertical | no | yes |  |  | `purpose`: `market_vertical`; `allowed_values_listed_in`: `campaign_vertical_records` |
| `eventnumber` | text |  | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |

## Sublist `campaigndirectmail` (Direct Mail)

| Field ID | Type | Label | Required | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|
| `cost` | currency | Cost | no | yes |  | `purpose`: `total_campaign_cost`; `value_source`: `base_cost_plus_event_costs`; `effect`: `used_in_roi`; `help_text_shared_with`: `campaign.cost`, `salescampaign.cost` |
| `campaigngroup` | select | Target Group | no |  |  |  |
| `channel` | select | Channel | yes |  |  |  |
| `datescheduled` | date | Date | no |  |  |  |
| `description` | text | Title | yes |  |  |  |
| `internalid` | integer |  | no |  |  |  |
| `promocode` | select | Promotion | no |  |  |  |
| `status` | select | Status | no |  |  |  |
| `subscription` | select | Subscription | no |  |  |  |
| `template` | select | Template | no |  |  |  |

## Sublist `campaignemail` (E-mail)

| Field ID | Type | Label | Required | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|
| `cost` | currency | Cost | no | yes |  | `purpose`: `total_campaign_cost`; `value_source`: `base_cost_plus_event_costs`; `effect`: `used_in_roi`; `help_text_shared_with`: `campaign.cost`, `salescampaign.cost` |
| `campaigngroup` | select | Target Group | yes |  |  |  |
| `channel` | select | Channel | yes |  |  |  |
| `datescheduled` | date | Date | yes |  |  |  |
| `description` | text | Title | yes |  |  |  |
| `internalid` | integer |  | no |  |  |  |
| `promocode` | select | Promotion | no |  |  |  |
| `status` | select | Status | no |  |  |  |
| `subscription` | select | Subscription | yes |  |  |  |
| `template` | select | Template | yes |  |  |  |
| `testcell` | select | Test Cell | no |  |  |  |
| `timescheduled` | timeofday | Time | no |  |  |  |

## Sublist `campaignevent` (Other Events)

| Field ID | Type | Label | Required | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|
| `cost` | currency | Cost | no | yes |  | `purpose`: `total_campaign_cost`; `value_source`: `base_cost_plus_event_costs`; `effect`: `used_in_roi`; `help_text_shared_with`: `campaign.cost`, `salescampaign.cost` |
| `campaigngroup` | select | Target Group | no |  |  |  |
| `channel` | select | Channel | yes |  |  |  |
| `datescheduled` | date | Date | no |  |  |  |
| `description` | text | Title | yes |  |  |  |
| `internalid` | integer |  | no |  |  |  |
| `promocode` | select | Promotion | no |  |  |  |
| `status` | select | Status | no |  |  |  |
| `subscription` | select | Subscription | no |  |  |  |

## Sublist `campaigndrip` (Lead Nurturing)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `channel` | select | Channel | yes |  |  |
| `cost` | currency | Cost | no |  |  |
| `description` | text | Title | yes |  |  |
| `internalid` | integer |  | no |  |  |
| `promocode` | select | Promotion | no |  |  |
| `subscription` | select | Subscription | yes |  |  |
| `template` | select | Template | yes |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `communication` | Communication |
| `custom` | Custom |
| `events` | Scheduled Events |
| `history` | History |
| `keyword` | Keyword |
| `media` | File Cabinet |
| `relatedinfo` | Related Info |
| `statistics` | Statistics |
| `workflow` | Workflow |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `audience` | select | Audience Description |  |
| `basecost` | currency | Cost |  |
| `campaigneventtype` | select | Campaign Event Type |  |
| `campaignid` | text | Campaign ID |  |
| `category` | select | Category |  |
| `channel` | select | Channel |  |
| `cost` | currency | Total Cost |  |
| `createddate` | datetime | Date Created |  |
| `enddate` | date | End Date |  |
| `event` | text | Campaign Event |  |
| `expectedrevenue` | currency | Expected Revenue |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `family` | select | Family |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `group` | select | Group |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `issalescampaign` | checkbox | Is Sales Campaign |  |
| `item` | select | Item |  |
| `keyword` | text | Keyword |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `manager` | select | Campaign Manager |  |
| `managerrole` | select | Campaign Manager Role |  |
| `number` | integer | Number |  |
| `offer` | select | Offer |  |
| `promocode` | select | Promotion |  |
| `recipient` | select | Recipient |  |
| `response` | select | Response Detail |  |
| `responsecategory` | select | Response |  |
| `responsecode` | integer | Response Code |  |
| `responsecomments` | text | Response Comments |  |
| `responsedate` | date | Response Date |  |
| `scheduledate` | date | Schedule Date |  |
| `searchengine` | select | Search Engine |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subscription` | select | Subscription |  |
| `template` | select | Campaign Templates |  |
| `title` | text | Title |  |
| `vertical` | select | Vertical |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `audience` | select | Audience Description |  |
| `basecost` | currency | Base Cost |  |
| `campaignid` | text | Campaign ID |  |
| `category` | select | Category |  |
| `channel` | select | Channel |  |
| `cost` | currency | Total Cost |  |
| `createddate` | datetime | Date Created |  |
| `enddate` | date | End Date |  |
| `event` | text | Event |  |
| `executeddate` | datetime | Executed on |  |
| `expectedrevenue` | currency | Expected Revenue |  |
| `externalid` | select | External ID |  |
| `family` | select | Family |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `issalescampaign` | checkbox | Is Sales Campaign |  |
| `item` | select | Item |  |
| `keyword` | text | Keyword |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `managerrole` | select | Manager Role |  |
| `message` | text | Description |  |
| `offer` | select | Offer |  |
| `owner` | select | Manager |  |
| `promocode` | text | Promotion |  |
| `recipient` | select | Recipient |  |
| `response` | select | Response Detail |  |
| `responsecategory` | select | Response |  |
| `responsecode` | integer | Response Code |  |
| `responsedate` | datetime | Response Date |  |
| `responsenotes` | textarea | Response Notes |  |
| `revenue` | currency | Revenue |  |
| `roi` | percent | ROI |  |
| `scheduleddate` | datetime | Scheduled on |  |
| `searchengine` | select | Search Engine |  |
| `startdate` | date | Start Date |  |
| `status` | select | Status |  |
| `subscription` | select | Subscription |  |
| `title` | text | Title |  |
| `url` | text | URL |  |
| `vertical` | select | Vertical |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `messages` | Messages | Message |
| `originatinglead` | Originating Lead | Lead |
