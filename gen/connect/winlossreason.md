# Winlossreason (`winlossreason`)
<!-- id: connect:winlossreason -->

| Fact | Value |
|---|---|
| Table | `winlossreason` |
| Record | `winlossreason` |
| Primary key | `win_loss_reason_id` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `date_last_modified` | TIMESTAMP |  |  |  |  | last_modified_column | `purpose`: `date_last_modified`; `time_zone`: `gmt` |
| `is_inactive` | VARCHAR2 | 3 |  |  |  |  | `purpose`: `is_inactive` |
| `reason` | VARCHAR2 | 50 |  |  |  |  | `purpose`: `reason` |
| `win_loss_reason_extid` | VARCHAR2 | 255 |  |  |  |  | `purpose`: `win_loss_reason_external_id` |
| `win_loss_reason_id` | NUMBER |  | 39 | 0 |  | primary_key | `purpose`: `win_loss_reason_id` |

## Referenced by

| Key | Table | Columns | Columns here |
|---|---|---|---|
| `Opportunities_winlossreason_fk` | `opportunities` | `win_loss_reason_id` | `win_loss_reason_id` |
