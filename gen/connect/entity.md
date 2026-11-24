# Entity (`entity`)
<!-- id: connect:entity -->

| Fact | Value |
|---|---|
| Table | `entity` |
| Record | `entity` |
| Primary key | `entity_id` |
| Domains | Campaignevents |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `address_one` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_one` |
| `address_three` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_two` |
| `address_two` | VARCHAR2 | 150 |  |  |  |  | `purpose`: `address_two` |
| `city` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `city` |
| `country` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `country` |
| `create_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `create_date`; `time_zone`: `gmt` |
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified` |
| `email` | VARCHAR2 | 254 |  |  |  |  | `purpose`: `email` |
| `entity_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `entity_external_id` |
| `entity_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `entity_id` |
| `entity_type` | VARCHAR2 | 12 |  |  |  |  | `purpose`: `entity_type` |
| `first_name` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `first_name` |
| `full_name` | VARCHAR2 | 1800 |  |  |  |  | `purpose`: `full_name` |
| `global_subscription_status` | NUMBER |  | 39 | 0 |  |  | `purpose`: `global_subscription_status` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `entity_is_inactive` |
| `is_online_bill_pay` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `enable_online_bill_pay` |
| `is_unavailable` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_unavailable` |
| `last_modified_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `last_name` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `last_name` |
| `login_access` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `login_access` |
| `middle_name` | VARCHAR2 | 32 |  |  |  |  | `purpose`: `middle_name` |
| `name` | VARCHAR2 | 83 |  |  |  |  | `purpose`: `name` |
| `notes` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `notes` |
| `originator_id` | VARCHAR2 | 40 |  |  |  |  | `purpose`: `originator_id` |
| `parent_id` | NUMBER |  | 39 | 0 |  |  | `purpose`: `parent_id` |
| `phone` | VARCHAR2 | 100 |  |  |  |  | `purpose`: `phone` |
| `salutation` | VARCHAR2 | 30 |  |  |  |  | `purpose`: `salutation` |
| `state` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `state` |
| `subsidiary` | NUMBER |  | 39 | 0 |  |  | `purpose`: `subsidiary` |
| `unsubscribed` | VARCHAR2 | 1 |  |  |  |  | `purpose`: `unsubscribed` |
| `zipcode` | VARCHAR2 | 36 |  |  |  |  | `purpose`: `zip_code` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Campaign_subscription_statuses_entity_fk` | `campaign_subscription_statuses` | `entity_id` | `entity_id` |
| `Campaignresponse_entity_fk` | `campaignresponse` | `entity_id` | `entity_id` |
| `Campaignresponsehistory_entity_fk` | `campaignresponsehistory` | `author` | `entity_id` |
| `Campaigns_entity_fk` | `campaigns` | `organizer_id` | `entity_id` |
| `Caseescalationhistory_entity_fk` | `caseescalationhistory` | `escalator_id` | `entity_id` |
| `Caseescalationhistory_entity_fk_2` | `caseescalationhistory` | `entity_id` | `entity_id` |
| `Contact_types_entity_fk` | `contact_types` | `contact_id` | `entity_id` |
| `Contact_types_entity_fk_2` | `contact_types` | `owner_id` | `entity_id` |
| `Currency_exchange_rates_entity_fk` | `currency_exchange_rates` | `entity_id` | `entity_id` |
| `Currencyrates_entity_fk` | `currencyrates` | `entity_id` | `entity_id` |
| `Customers_entity_fk` | `customers` | `converted_to_id` | `entity_id` |
| `Customers_entity_fk_2` | `customers` | `converted_to_contact_id` | `entity_id` |
| `Customers_entity_fk_3` | `customers` | `sales_rep_id` | `entity_id` |
| `Customers_entity_fk_4` | `customers` | `parent_id` | `entity_id` |
| `Entity_role_map_entity_fk` | `entity_role_map` | `entity_id` | `entity_id` |
| `Entityterritorymap_entity_fk` | `entityterritorymap` | `entity_id` | `entity_id` |
| `Messagerecipient_entity_fk` | `messagerecipient` | `entity_id` | `entity_id` |
| `Notes_user_entity_fk` | `notes_user` | `company_id` | `entity_id` |
| `Opportunities_entity_fk` | `opportunities` | `sales_rep_id` | `entity_id` |
| `Opportunity_lines_entity_fk` | `opportunity_lines` | `company_id` | `entity_id` |
| `Opportunity_lines_entity_fk_2` | `opportunity_lines` | `related_company_id` | `entity_id` |
| `Resource_group_entity_map_entity_fk` | `resource_group_entity_map` | `entity_id` | `entity_id` |
| `Resource_groups_entity_fk` | `resource_groups` | `resource_group_id` | `entity_id` |
| `Support_incidents_entity_fk` | `support_incidents` | `owner_id` | `entity_id` |
| `Support_incidents_entity_fk_2` | `support_incidents` | `assigned_id` | `entity_id` |
| `Support_incidents_entity_fk_3` | `support_incidents` | `company_id` | `entity_id` |
| `Supportcasehistory_entity_fk` | `supportcasehistory` | `employee_id` | `entity_id` |
| `System_notes_custom_entity_fk` | `system_notes_custom` | `company_id` | `entity_id` |
| `System_notes_custom_entity_fk_2` | `system_notes_custom` | `author_id` | `entity_id` |
| `System_notes_custom_entity_fk_3` | `system_notes_custom` | `customfield_created_by_id` | `entity_id` |
| `System_notes_custom_entity_fk_4` | `system_notes_custom` | `customfield_modified_by_id` | `entity_id` |
| `System_notes_entity_fk` | `system_notes` | `company_id` | `entity_id` |
| `System_notes_entity_fk_2` | `system_notes` | `author_id` | `entity_id` |
| `Transaction_address_entity_fk` | `transaction_address` | `created_by_id` | `entity_id` |
| `Transaction_address_entity_fk_2` | `transaction_address` | `last_modified_by_id` | `entity_id` |
| `Transaction_history_entity_fk` | `transaction_history` | `user_id` | `entity_id` |
| `Transaction_history_entity_fk_2` | `transaction_history` | `entity_id` | `entity_id` |
