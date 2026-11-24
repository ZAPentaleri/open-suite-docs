# HCMJob (`hcmjob`)
<!-- id: record:hcmjob -->

| Fact | Value |
|---|---|
| Internal ID | `hcmjob` |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `description` | textarea | Description | no | no |  |  | `purpose`: `job_description`; `constraint`: `unique` |
| `employmentcategory` | select | Employment Category | no |  |  |  | `purpose`: `employment_category`; `allowed_values`: `Full-Time`, `Part-Time` |
| `isinactive` | checkbox | Inactive | no |  |  |  | `purpose`: `inactivate_job`; `ui_effect`: `changes_jobs_list_display` |
| `jobid` | text | Job ID | no | no | yes |  | `purpose`: `job_id`; `value_source`: `system_generated` |
| `title` | text | Title | yes | no |  |  | `purpose`: `job_title`; `constraint`: `unique` |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |  |
| `inactive` | checkbox | Inactive | no | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `history` | System Notes |
