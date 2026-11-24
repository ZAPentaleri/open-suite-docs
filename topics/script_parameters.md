# Creating Script Parameters
<!-- id: topic:script_parameters -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [Custom fields](../topics/custom_fields.md), [Script records](script_record.md),
[N/runtime](../modules/N_runtime.md), [N/task](../modules/N_task.md),
[Script Deployment](script_deployment.md).

Script parameters are settings attached to a script so its behaviour can change without a code edit.
Despite the name they are not function arguments: they behave like custom fields, defined on the
script record and given values per deployment, per company or per user.

## Why use them

- Make one script configurable. For example, one script deployed on 50 records can behave differently
  on each, driven by its parameter values.
- Configure each deployment separately, scheduled script deployments included.
- Let administrators adjust a bundled script without touching code, and change behaviour while
  troubleshooting without a code change.
- Let people who install a bundle tailor the script through preferences.

Do not put confidential information in parameters: search engines can index their values.

## Creating a parameter

1. Edit the script record, open the **Parameters** subtab and click **New Parameter**.
2. Enter a **Label**. It is shown as the field label wherever the deployed script's parameters appear
   in the UI.
3. Enter an **ID**: lowercase, no spaces, at most 30 characters. `custscript` is placed in front of
   it. Prefer your own ID over a generated one, because it avoids conflicts when the script is added to
   a SuiteCloud project. Note the ID, since your code refers to it.
4. Choose a **Type** (see below). For a list/record type, also pick the list or record; for saved
   searches, only public ones can be chosen.
5. Choose a **Preference** (see below).
6. Fill in the optional subtabs as needed: Display, Validation & Defaulting, Sourcing & Filtering,
   Access and Translation.
7. Save the parameter, then save the script record.

## Parameter types

Parameters take the same field types as [custom fields](../topics/custom_fields.md): check box,
currency, date, date/time, decimal number, document, email address, free-form text, help, hyperlink,
image, inline HTML, integer number, list/record, long text, multiple select, password, percent, phone
number, rich text, text area, time of day and URL fragment. Length limits for the text types are on
the custom fields page.

## Preference: where the value comes from

| Preference | Where the value is set |
|---|---|
| Company | The General Preferences page, Custom Preferences subtab. Becomes the default for the company. |
| User | The Set Preferences page, Custom Preferences subtab. Users can override the company default without a code change. |
| Portlet | The portlet's setup. |
| None | On the deployment record's Parameters subtab, as a deployment parameter. This is the default behaviour when no preference is chosen. |

Pick company or user when the people using the script are expected to change the value.

For example, a check box parameter with a company preference is checked for everybody once the
company preference holds `T`.

## Bundles

- Parameters with a company or user preference are not overwritten by a bundle update.
- Deployment parameters follow the bundle's object preference, **Update Deployments** or **Do Not
  Update Deployments**. Choose Do Not Update Deployments to keep the values already set in the target
  account.

## Reading and setting values from code

- Read a value with [`runtime.getCurrentScript()`](../modules/N_runtime.md) and
  [`Script.getParameter()`](../modules/N_runtime.md); a Suitelet, for instance, can read a check box
  parameter this way.
- A script cannot write parameter values back.
- Outside the UI, values for a scheduled script run can be passed through the `params` option of
  [`task.create`](../modules/N_task.md); they override the deployment's stored values for that run.
- The parameter must first be created in the UI on the script record; code cannot define one.

## Example

A scheduled script whose cut-off age and dry-run switch come from deployment parameters, so the same
code can run as a cautious test deployment and as a live one. The parameter IDs are invented; the
check for `'T'` is a defensive JavaScript habit, not a documented return format.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/runtime', 'N/log'], (runtime, log) => {
  const execute = () => {
    const script = runtime.getCurrentScript();
    const maxAgeDays = Number(script.getParameter({ name: 'custscript_quote_max_age_days' }) || 30);
    const dryRunRaw = script.getParameter({ name: 'custscript_quote_dry_run' });
    const dryRun = dryRunRaw === true || dryRunRaw === 'T';

    log.audit({
      title: 'Quote clean-up settings',
      details: `Close quotes older than ${maxAgeDays} days${dryRun ? ' (dry run)' : ''}`
    });
  };

  return { execute };
});
```
