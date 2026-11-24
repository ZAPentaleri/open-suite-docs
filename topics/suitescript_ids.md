# SuiteScript IDs
<!-- id: topic:suitescript_ids -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

Sub-topics:

- [Preference Names and IDs](suitescript_ids.preferences.md)

Some SuiteScript APIs expect the identifier of something built into NetSuite rather than a record or
field ID. Five kinds of such IDs exist: permission IDs, feature IDs, preference IDs, task IDs and
button IDs. Preference IDs are covered on their own page (see the sub-topic above); the other four are
described here.

## Permission IDs

A permission ID is what you pass as the `name` option of
[`User.getPermission`](../modules/N_runtime.md) (call it on the object that
`runtime.getCurrentUser()` returns) to learn the current user's level for that permission.

Valid IDs are listed in [catalog:permissions](../gen/catalogs/permissions.md). Each row gives the
permission ID, the permission's name as shown in NetSuite, the feature (group) it belongs to, and the
levels that are valid for it.

A NetSuitePermissionsUsage worksheet is available that shows where each permission is used.

Some permissions appear in the source under a second name, sometimes in a different group. The
catalog keeps the first listing and records the other one; both names refer to the same ID:

| ID | Name | Also listed as (name / group / levels) |
|---|---|---|
| `ADMI_ACCTSETUP` | Set Up Accounting | Accounting Preferences / Accounting / none, full |
| `ADMI_BUNDLER` | SuiteBundler | SuiteApp Marketplace / SuiteBundler / none, full |
| `ADMI_CLASSSEGMENTMAPPING` | Class Segment Mapping | Class Mapping / Multi Book Version 2 / levels not given |
| `ADMI_DEPTSEGMENTMAPPING` | Department Segment Mapping | Department Mapping / Multi Book Version 2 / none, full |
| `ADMI_LOCSEGMENTMAPPING` | Location Segment Mapping | Location Mapping / Multi Book Version 2 / none, full |
| `ADMI_PROJECT_ACCOUNTING_SETUP` | Project Profitability Setup | Project Profitability / Advanced Project Profitability / none, full |
| `ADMI_SUITE_OAX_CONNECTOR` | NSAW Connector Administrator | OAX Connector Administrator / NetSuite Analytics Warehouse / none, full |
| `ADMI_WEBSERVICES` | SOAP Web Services | Web Services / Internal Web Services / none, full |
| `ADMI_WEBSERVICESLOG` | View SOAP Web Services Logs | View Web Services Logs / Internal Web Services / none, full |
| `ADMI_WEBSERVICESSETUP` | Set Up SOAP Web Services | Set Up Web Services / Internal Web Services / none, full |
| `LIST_EMPLOYEECHANGETYPE` | Employee Change Request Type | Employee Change Request Type / Employee Change Requests / none, view, create, edit, full |
| `REPO_SALES_PROMO` | Sales By Promotion Code | Sales By Promotion / Sales Force Automation / none, view |

## Feature IDs

A feature ID is passed to [`runtime.isFeatureInEffect`](../modules/N_runtime.md) to find out whether
a feature is enabled in the account. The valid IDs are in
[catalog:features](../gen/catalogs/features.md).

## Button IDs

Standard buttons have IDs, listed in [catalog:buttons](../gen/catalogs/buttons.md).

Standard buttons are shown either inline on the form or inside the Actions menu. Through the
[`serverWidget.Button`](../modules/N_ui_serverWidget.md) object a script can adjust two things about a
standard button:

- its text, through [`Button.label`](../modules/N_ui_serverWidget.md);
- its visibility, through [`Button.isHidden`](../modules/N_ui_serverWidget.md).

Limits on what a script can do:

- It cannot move a button between the inline row and the Actions menu.
- It cannot add buttons to the Actions menu or take buttons out of it.
- The Save, Edit, Cancel, Back and Reset buttons cannot be customized at all.

If you need to change whether a button sits inline or in the Actions menu, do it with point-and-click
form customization rather than in a script.

## Task IDs

A task ID identifies a NetSuite page. Two APIs take one:

- [`url.resolveTaskLink`](../modules/N_url.md) returns the page's URL;
- [`redirect.toTaskLink`](../modules/N_redirect.md) sends the user to the page.

`LIST_EMPLOYEE_RECORD` is an example of a task ID. The source does not provide a list of task IDs.
To find the ID of a particular page:

1. Open the page and view its HTML source.
2. Search the source for `taskId`.
3. Read the value given for it.

## Example

A `beforeLoad` user event on sales orders. In accounts that use multiple currencies, users without
full Accounting Management access see the standard Approve button relabelled, and the script logs the
URL of the employee list page. The permission, feature and button IDs come from the catalogs linked
above.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/runtime', 'N/url', 'N/log'], (runtime, url, log) => {
  const beforeLoad = (context) => {
    if (context.type !== context.UserEventType.VIEW) {
      return;
    }
    if (!runtime.isFeatureInEffect({ feature: 'MULTICURRENCY' })) {
      return;
    }
    const level = runtime.getCurrentUser().getPermission({ name: 'ADMI_ACCOUNTING' });
    if (level !== runtime.Permission.FULL) {
      const approveBtn = context.form.getButton({ id: 'approve' });
      if (approveBtn) {
        approveBtn.label = 'Request FX Approval';
      }
    }
    const staffListUrl = url.resolveTaskLink({ id: 'LIST_EMPLOYEE_RECORD' });
    log.debug({ title: 'Employee list', details: staffListUrl });
  };

  return { beforeLoad };
});
```
