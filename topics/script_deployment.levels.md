# Deployment levels
<!-- id: topic:script_deployment.levels -->

Part of: [Script deployment](script_deployment.md)

See also: [Record-level deployment](script_deployment.record_level.md),
[Form-level deployment](script_deployment.form_level.md), [ClientScript](../script-types/ClientScript.md).

A script can be deployed at record level or at form level. Only
[client scripts](../script-types/ClientScript.md) can use form level.

| | Record level | Form level |
|---|---|---|
| Where it runs | On every form of the record types it is deployed to | Only when that particular custom form is used |
| Can be limited to one form | No | Yes, that is its purpose |
| Audience limits (role, group or other classification, on the Audience subtab) | Yes | No |
| Attach to | Record types | Custom entry, transaction or address forms |
| Client scripts on Suitelet-generated forms and lists | Work at record level | Suitelets cannot use form-level scripts |

Whatever the level, a client script cannot read form fields that are hidden.
