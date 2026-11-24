# Web store impact
<!-- id: topic:script_deployment.web_store_impact -->

Part of: [Script deployment](script_deployment.md)

See also: [UserEventScript](../script-types/UserEventScript.md), [Script deployment](script_deployment.md).

Scripts that fire during web store activity can make the store slow. Two execution contexts belong
to the web store: **Web Application** and **Web Store**.

## Keeping scripts out of the store

On the Context Filtering subtab of a deployment, for example of a
[user event script](../script-types/UserEventScript.md), leave the two web store contexts unselected.
Any context you do not select is switched off for that deployment.

## Asynchronous alternative

The **asynchronous afterSubmit sales order processing** feature is another option. With it, the
afterSubmit user event scripts and the workflows triggered by a web checkout run asynchronously.
