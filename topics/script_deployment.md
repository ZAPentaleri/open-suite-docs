# Script deployment
<!-- id: topic:script_deployment -->

Sub-topics:

- [Entry point script validation](entry_point_validation.md)
- [Setting runtime options](runtime_options.md)
- [Deployment levels](script_deployment.levels.md)
- [Record-level deployment](script_deployment.record_level.md)
- [Form-level deployment](script_deployment.form_level.md)
- [Deployment record](script_deployment.deployment_record.md)
- [Deployments sublist](script_deployment.deployments_sublist.md)
- [Script system notes](script_deployment.system_notes.md)
- [Web store impact](script_deployment.web_store_impact.md)

See also: [Script records](script_record.md), [scriptdeployment record](../gen/records/scriptdeployment.md),
[ClientScript](../script-types/ClientScript.md), [N/record](../modules/N_record.md).

Writing a script file is only the first step. Getting it to run takes three: upload the file to the
File Cabinet, create a [script record](script_record.md) for it, and deploy it. The file must pass
[entry point validation](entry_point_validation.md) before any of that works. A script without a
deployment never runs.

## Two levels of deployment

- **Record level** works for every script type that has entry points (see
  [Record-level deployment](script_deployment.record_level.md)).
- **Form level** is only for [client scripts](../script-types/ClientScript.md) (see
  [Form-level deployment](script_deployment.form_level.md)).

[Deployment levels](script_deployment.levels.md) compares the two.

## Deployment records

- Deploying creates a [script deployment record](../gen/records/scriptdeployment.md). You can create
  it together with the script record or later.
- A script can have several deployments. They run in the order they are listed on the script
  record's Deployments subtab, which is usually the order in which they were created.
- Deployments are listed at Customization > Scripting > Script Deployments and on the Deployments
  subtab of the script record.
- Working with deployments needs the SuiteScript permission at Edit level.
- Which settings a deployment offers depends on the script type and on how you deploy.

## Three ways to deploy

| Method | Notes |
|---|---|
| Deployments subtab on the script record | Quick, but does not show every deployment field, and for some script types it is read-only while you edit the script record. See [Deployments sublist](script_deployment.deployments_sublist.md). |
| Script deployment record | Shows more fields than the sublist. See [Deployment record](script_deployment.deployment_record.md). |
| N/record in a script | Create with [`record.create`](../modules/N_record.md#recordcreateoptions) using type `record.Type.SCRIPT_DEPLOYMENT`; edit with [`record.load`](../modules/N_record.md#recordloadoptions). Field IDs are on the [scriptdeployment](../gen/records/scriptdeployment.md) page. |

In most cases the same methods can also be used to edit existing deployments.

## Context filtering

The Context Filtering subtab limits a deployment by two kinds of context:

- **Execution context**: how the script was triggered.
- **Localization context**: the countries or regions where the script runs, worked out from
  country fields on the record such as subsidiary and tax nexus.

See also [Web store impact](script_deployment.web_store_impact.md).

## Example

A Suitelet helper that switches a deployment off by clearing its **Deployed** box (field
`isdeployed`), the documented way to stop a deployment from running. The deployment's internal ID
comes in as a request parameter.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/record'], (record) => {
  const onRequest = (context) => {
    const deploymentId = context.request.parameters.deployment;
    const deployment = record.load({ type: record.Type.SCRIPT_DEPLOYMENT, id: deploymentId });
    const title = deployment.getValue({ fieldId: 'title' });
    deployment.setValue({ fieldId: 'isdeployed', value: false });
    deployment.save();
    context.response.write(`Deployment "${title}" is no longer deployed.`);
  };

  return { onRequest };
});
```
