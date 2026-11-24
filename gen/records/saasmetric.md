# SaaS Metric (`saasmetric`)
<!-- id: record:saasmetric -->

| Fact | Value |
|---|---|
| Internal ID | `saasmetric` |
| Category | item |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_read`, `supports_search` |

## Record facts

- `requires_feature`: `SaaS Metrics` [`real_name`]
- `script_functions`: `read`, `search`
- `doc_inconsistency`: `says_not_supported_in_server_or_client`
- `purpose`: `saas_metrics_reporting`
- `configured_by`: `enable_features_analytics_subtab` [`saas_metrics`]
- `can`: `past_start_date_for_historical_reporting`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `acquisitionmonthenddate` | date | Acquisition Month End Date | no |  | marked_new |  |
| `beginningarrbalance` | currency | Beginning Balance ARR | no |  |  |  |
| `beginningmrrbalance` | currency | Beginning Balance MRR | no |  |  |  |
| `class` | select | Class | no |  |  |  |
| `department` | select | Department | no |  |  |  |
| `enddate` | date | End Date | no |  |  |  |
| `endingarrbalance` | currency | Ending Balance ARR | no |  |  |  |
| `endingmrrbalance` | currency | Ending Balance MRR | no |  |  |  |
| `entity` | select | Customer | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `item` | select | Item | no |  |  |  |
| `lastupdated` | date | Last Updated | no |  |  |  |
| `linkedanalyticalimpact` | select | Linked Analytical Impact | no |  |  |  |
| `location` | select | Location | no |  |  |  |
| `revenuemonthenddate` | date | Revenue Month End Date | no |  |  |  |
| `source` | text | Source | no |  |  |  |
| `sourcecurrency` | select | Source Currency | no |  |  |  |
| `sourceid` | integer | Source ID | no |  |  |  |
| `sourceline` | text | Source Line | no |  |  |  |
| `sourcelineid` | integer | Source Line ID | no |  |  |  |
| `sourcerecordtype` | select | Source Record Type | no |  |  |  |
| `startdate` | date | Start Date | no |  |  |  |
| `subsidiary` | select | Subsidiary | no |  |  |  |
| `terminmonths` | bigdecimal | Term In Months | no |  |  |  |

## Sublist `saasmetricimpact` (SaaS Metric Event List)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `arr` | currency | ARR | no |  |  |
| `eventnumber` | integer | Event Number | no |  |  |
| `id` | integer | ID | no |  |  |
| `impactcategory` | select | Impact Category | no |  |  |
| `impactsubcategory` | select | Impact Subcategory | no |  |  |
| `linkedanalyticalimpactevent` | select | Linked Analytical Impact Event | no |  |  |
| `mrr` | currency | MRR | no |  |  |
| `saasmetric` | select | SaaS Metric | no |  |  |

## Suspected source errors

> [!SOURCE-ERROR] `says_not_supported_in_server_or_client`
<!-- source-error: record:saasmetric | - | says_not_supported_in_server_or_client -->
