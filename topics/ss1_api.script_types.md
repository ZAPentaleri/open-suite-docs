# SuiteScript 1.0 script types
<!-- id: topic:ss1_api.script_types -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; each 1.0 type below links to its 2.1 counterpart.

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## The nine 1.0 script types

| 1.0 type | Runs on | 1.0 details | 2.1 counterpart |
|---|---|---|---|
| User event | Server | [User event scripts](ss1_api.user_events.md) | [UserEventScript](../script-types/UserEventScript.md) |
| Suitelet | Server | [Suitelets](ss1_api.suitelets.md) | [Suitelet](../script-types/Suitelet.md) |
| RESTlet | Server | [RESTlets](ss1_api.restlets.md) | [Restlet](../script-types/Restlet.md) |
| Scheduled | Server | [Scheduled scripts](ss1_api.scheduled_scripts.md) | [ScheduledScript](../script-types/ScheduledScript.md) |
| Client | Browser | [Client scripts](ss1_api.client_scripts.md) | [ClientScript](../script-types/ClientScript.md) |
| Portlet | Server | [Portlet scripts](ss1_api.portlets.md) | [Portlet](../script-types/Portlet.md) |
| Mass update | Server | [Mass update scripts](ss1_api.mass_update.md) | [MassUpdateScript](../script-types/MassUpdateScript.md) |
| Workflow action | Server | [Workflow action scripts](ss1_api.workflow_action.md) | [WorkflowActionScript](../script-types/WorkflowActionScript.md) |
| Bundle installation | Server | [Bundle installation scripts](ss1_api.bundle_installation.md) | [BundleInstallationScript](../script-types/BundleInstallationScript.md) |

Client scripts are the only 1.0 type that runs on the client; every other type runs on the server.

## Two ways to deploy a client script

- **Form-level:** the script is attached to a particular form and runs only there.
- **Record-level:** the script is deployed to a record type and runs on that type's forms.

The differences are covered in [SuiteScript 1.0 client scripts](ss1_api.client_scripts.md).
