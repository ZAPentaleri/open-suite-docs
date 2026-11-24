# Sales Campaign (`salescampaign`)
<!-- id: record:salescampaign -->

| Fact | Value |
|---|---|
| Internal ID | `salescampaign` |
| Category | marketing |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `requires_feature`: `Marketing Automation`, `Sales Campaigns` [`real_name`]
- `not_supported`: `transform`
- `read_only_fields`: `totalRevenue`, `roi`, `profit`, `costPerCustomer`, `convCostPerCustomer`, `conversions`, `leadsGenerated`, `uniqueVisitors`, `cost`, `campaignId`
- `read_only_sublists`: `defaultEvent`
- `sublists`: `campaignDirectMail`, `campaignDrip`, `campaignEmail`, `campaignEvent`, `defaultEvent`
- `purpose`: `refine_sales_campaign_of_marketing_campaign`
- `contains`: `audience`, `category`, `family`, `offer`, `owner`, `ownerRole`, `promotionCode`, `searchEngine`, `vertical` [`subrecords`]
- `requires_feature`: `Promotion Codes`, `Subscription Categories`, `Mail Merge` [`needed_for_all_fields`, `real_name`]
- `effect`: `statistics_fields_computed_automatically`
- `effect`: `cost_computed_from_event_costs`
- `effect`: `campaignid_and_defaultevent_generated_after_create`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `audience` | select | Audience | no |  |  | marked_new | `purpose`: `campaign_audience`; `allowed_values_listed_in`: `campaign_audience_records`; `use_when`: `media_marketing_without_recipient_list` |
| `basecost` | currency | Cost | no |  |  | marked_new | `purpose`: `campaign_base_cost`; `effect`: `added_to_event_costs_for_total` |
| `category` | select | Category | no |  |  | marked_new | `purpose`: `campaign_category`; `allowed_values_listed_in`: `campaign_category_records` |
| `cost` | currency | Total Cost | no |  | yes | marked_new | `purpose`: `total_campaign_cost`; `value_source`: `base_cost_plus_event_costs`; `effect`: `used_in_roi`; `help_text_shared_with`: `campaign.campaigndirectmail.cost`, `campaign.campaignemail.cost`, `campaign.campaignevent.cost` |
| `customform` | select | Custom Form | no |  |  | marked_new | `purpose`: `campaign_form`; `ui_effect`: `new_option_creates_record` |
| `enddate` | date | End Date | no |  |  | marked_new | `purpose`: `campaign_end_date` |
| `expectedrevenue` | currency | Projected Income | no |  |  | marked_new | `purpose`: `expected_revenue` |
| `family` | select | Family | no |  |  | marked_new | `purpose`: `keyword_family`; `allowed_values_listed_in`: `campaign_family_records`; `value_kind`: `grouping_of_paid_keywords` |
| `item` | multiselect | Item | no |  |  | marked_new | `purpose`: `items_offered`; `value_kind`: `multi_select`; `supported_in`: `search` |
| `keyword` | text | Keyword | no |  |  | marked_new | `purpose`: `paid_keyword` |
| `message` | textarea | Description | no |  |  | marked_new | `purpose`: `campaign_description`; `displayed_on`: `campaign_record_only` |
| `offer` | select | Offer | no |  |  | marked_new | `purpose`: `campaign_offer`; `allowed_values_listed_in`: `campaign_offer_records` |
| `owner` | select | Campaign Manager | yes |  |  | marked_new | `purpose`: `campaign_manager`; `effect`: `notified_on_successful_event_delivery` |
| `ownerrole` | select | Manager Role | no |  |  | marked_new | `purpose`: `owner_role`; `value_source`: `system_displayed`; `constraint`: `editable_only_by_manager_in_this_role` |
| `promotioncode` | select | Promotion | no |  |  | marked_new | `purpose`: `promotion_code`; `allowed_values_listed_in`: `promotion_code_records`; `effect`: `offers_discounts_via_campaign`; `requires`: `recipients_enter_code_in_web_store` |
| `searchengine` | select | Search Engine | no |  |  | marked_new | `purpose`: `keyword_search_engine`; `allowed_values_listed_in`: `campaign_search_engine_records` |
| `startdate` | date | Start Date | no |  |  | marked_new | `purpose`: `campaign_start_date` |
| `title` | text | Title | yes |  |  | marked_new | `purpose`: `campaign_title`; `max_length`: `99` characters |
| `url` | url | URL | no |  |  | marked_new | `purpose`: `landing_page_url`; `effect`: `leadsource_parameter_appended` (when `campaign_id_or_autonumber`) |
| `vertical` | select | Vertical | no |  |  | marked_new | `purpose`: `market_vertical`; `allowed_values_listed_in`: `campaign_vertical_records` |
| `autoname` | checkbox | Auto | no |  |  | marked_new |  |
| `campaignid` | identifieranycase | ID | no |  |  | marked_new |  |
| `convcostpercustomer` | currency | Cost per Purchaser | no |  |  | marked_new |  |
| `conversions` | integer | Number of Purchasers | no |  |  | marked_new |  |
| `costpercustomer` | currency | Cost Per Lead | no |  |  | marked_new |  |
| `eventnumber` | integer | Number | no |  |  | marked_new |  |
| `externalid` | text | External ID | no |  |  | marked_new |  |
| `id` | integer | Internal ID | no |  |  | marked_new |  |
| `isdefaultleadsource` | checkbox | Default Lead Source | no |  |  | marked_new |  |
| `isinactive` | checkbox | Inactive | no |  |  | marked_new |  |
| `issalescampaign` | checkbox | Sales Campaign | no |  |  | marked_new |  |
| `leadsgenerated` | integer | Leads Generated | no |  |  | marked_new |  |
| `local` | checkbox | Execute On Local Server | no |  |  | marked_new |  |
| `origbinactive` | checkbox | Is Inactive (original) | no |  |  | marked_new |  |
| `profit` | currency | Profit | no |  |  | marked_new |  |
| `roi` | percent | Return on Investment | no |  |  | marked_new |  |
| `totalrevenue` | currency | Revenue | no |  |  | marked_new |  |
| `uniquevisitors` | integer | Unique Visitors | no |  |  | marked_new |  |

## Sublist `campaigndirectmail` (Direct Mail)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `campaign` | select | Campaign | no | marked_new |  |
| `campaigngroup` | select | Target Group | no | marked_new |  |
| `channel` | select | Channel | yes | marked_new |  |
| `cost` | currency | Cost | no | marked_new |  |
| `datescheduled` | date | Date | no | marked_new |  |
| `description` | text | Title | yes | marked_new |  |
| `dscheduled` | datetime | Date | no | marked_new |  |
| `externalid` | text | External ID | no | marked_new |  |
| `id` | integer | Internal ID | no | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | marked_new |  |
| `promocode` | select | Promotion | no | marked_new |  |
| `status` | select | Status | no | marked_new |  |
| `subscription` | select | Subscription | no | marked_new |  |
| `template` | select | Template | no | marked_new |  |

## Sublist `campaigndrip` (Lead Nurturing)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `campaign` | select | Campaign | no | marked_new |  |
| `channel` | select | Channel | yes | marked_new |  |
| `cost` | currency | Cost | no | marked_new |  |
| `description` | text | Title | yes | marked_new |  |
| `externalid` | text | External ID | no | marked_new |  |
| `id` | integer | Internal ID | no | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | marked_new |  |
| `promocode` | select | Promotion | no | marked_new |  |
| `subscription` | select | Subscription | yes | marked_new |  |
| `template` | select | Template | yes | marked_new |  |

## Sublist `campaignemail` (Email Campaign Event)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `campaign` | select | Campaign | no | marked_new |  |
| `campaigngroup` | select | Target Group | yes | marked_new |  |
| `channel` | select | Channel | yes | marked_new |  |
| `cost` | currency | Cost | no | marked_new |  |
| `datescheduled` | date | Date | yes | marked_new |  |
| `description` | text | Title | yes | marked_new |  |
| `dscheduled` | datetime | Date | no | marked_new |  |
| `externalid` | text | External ID | no | marked_new |  |
| `id` | integer | Internal ID | no | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | marked_new |  |
| `promocode` | select | Promotion | no | marked_new |  |
| `status` | select | Status | no | marked_new |  |
| `subscription` | select | Subscription | yes | marked_new |  |
| `template` | select | Template | yes | marked_new |  |
| `testcell` | select | Test Cell | no | marked_new |  |
| `timescheduled` | timeofday | Time | no | marked_new |  |

## Sublist `campaignevent` (Print/Other)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `campaign` | select | Campaign | no | marked_new |  |
| `campaigngroup` | select | Target Group | no | marked_new |  |
| `channel` | select | Channel | yes | marked_new |  |
| `cost` | currency | Cost | no | marked_new |  |
| `datescheduled` | date | Date | no | marked_new |  |
| `description` | text | Title | yes | marked_new |  |
| `dscheduled` | datetime | Date | no | marked_new |  |
| `externalid` | text | External ID | no | marked_new |  |
| `id` | integer | Internal ID | no | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | marked_new |  |
| `promocode` | select | Promotion | no | marked_new |  |
| `status` | select | Status | no | marked_new |  |
| `subscription` | select | Subscription | no | marked_new |  |

## Sublist `defaultevent` (Default)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `campaign` | select | Campaign | no | marked_new |  |
| `datescheduled` | date | Date | no | marked_new |  |
| `description` | text | Title | yes | marked_new |  |
| `dscheduled` | datetime | Date | no | marked_new |  |
| `externalid` | text | External ID | no | marked_new |  |
| `id` | integer | Internal ID | no | marked_new |  |
| `lastmodifieddate` | datetime | Last Modified Date | no | marked_new |  |
| `status` | text | Status | no | marked_new |  |
