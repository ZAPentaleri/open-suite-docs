# Caseescalationhistory (`caseescalationhistory`)
<!-- id: connect:caseescalationhistory -->

| Fact | Value |
|---|---|
| Table | `caseescalationhistory` |

## Columns

| Column | Type | Length | Precision | Scale | References | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `case_id` | NUMBER |  | 39 | 0 | `support_incidents`.`case_id` | foreign_key | `purpose`: `support_case_id` |
| `entity_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `entity_id` |
| `escalation_action` | VARCHAR2 | 10 |  |  |  |  | `purpose`: `escalation_action` |
| `escalation_date` | TIMESTAMP |  |  |  |  |  | `purpose`: `escalation_date` |
| `escalator_id` | NUMBER |  | 39 | 0 | `entity`.`entity_id` | foreign_key | `purpose`: `escalate_to` |
| `message` | VARCHAR2 | 4000 |  |  |  |  | `purpose`: `message` |

## Foreign keys

| Key | Columns | References table | Referenced columns |
|---|---|---|---|
| `caseescalationhistory_entity_fk` | `escalator_id` | `entity` | `entity_id` |
| `caseescalationhistory_entity_fk_2` | `entity_id` | `entity` | `entity_id` |
| `caseescalationhistory_support_incidents_fk` | `case_id` | `support_incidents` | `case_id` |
