# Script system notes
<!-- id: topic:script_deployment.system_notes -->

Part of: [Script deployment](script_deployment.md)

See also: [scriptdeployment record](../gen/records/scriptdeployment.md), [Script records](script_record.md).

Both script records and [script deployment records](../gen/records/scriptdeployment.md) keep a change
history on their **System Notes** subtab (open the record in view mode). It replaces the old History
subtab, which is no longer updated.

## What each note records

Date, Set By, Context, Type of change, Field, Old Value, New Value and Role.

## Who appears as Set By

| How the script was started | Set By | Notes |
|---|---|---|
| Save and Execute, or a schedule | System | |
| An internal Suitelet, a RESTlet or a user event script | The user who triggered it | That user is also recorded as creator or modifier. |
| An external Suitelet | — | The attempt fails with an insufficient privileges error. |
