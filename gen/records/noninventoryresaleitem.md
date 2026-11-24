# Non-inventory Item for Resale (`noninventoryresaleitem`)
<!-- id: record:noninventoryresaleitem -->

| Fact | Value |
|---|---|
| Internal ID | `noninventoryresaleitem` |
| Search data via | Item |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `directrevenueposting` | checkbox | Direct Revenue Posting | no |  |  | `purpose`: `bypass_advanced_revenue_management`; `true_means`: `posts_directly_to_revenue_no_arrangement`; `ui_effect`: `disables_deferred_revenue_account`; `constraint`: `all_transaction_items_must_match`; `constraint`: `all_kit_members_must_match`; `cannot`: `change_after_used_with_arm`; `default_value`: `false` |
| `revrecforecastrule` | select | Rev Rec Forecast Rule | yes |  |  | `purpose`: `forecast_revenue_rule`; `default_behavior`: `same_preference_as_revenue_rule`; `not_supported_for`: `percent_complete_rules` |
| `tracksaasmetrics` | checkbox | Track SaaS Metrics | no |  |  |  |
| `tracksaasmetricsenddate` | date | SaaS Metric End Date | no |  |  |  |
| `tracksaasmetricsstartdate` | date | SaaS Metric Start Date | no |  |  |  |
