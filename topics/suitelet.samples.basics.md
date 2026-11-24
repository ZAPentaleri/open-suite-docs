# Basic Suitelet Samples
<!-- id: topic:suitelet.samples.basics -->

Part of: [Suitelet Samples](suitelet.samples.md)

See also: [Suitelet](../script-types/Suitelet.md), [UserEventScript](../script-types/UserEventScript.md),
[Custom pages and assistants](custom_pages.assistants.md), [globals: `define` and `require`](../globals.md).

The basic group covers the shapes most Suitelets start from:

- **Custom assistants**: multi-step, wizard-style pages built with serverWidget (see
  [assistants](custom_pages.assistants.md)).
- **Suitelets with embedded HTML**: pages that mix serverWidget components with your own markup.
- **A user event script that adds a link to a Suitelet** on a subtab of a record.
- **A Suitelet that returns a small XML document** instead of a page.
- **A minimal page** that just writes text to the response.

## define for deployment, require for the debugger

Entry-point scripts, including every Suitelet, must be written with
[`define`](../globals.md). The only time to switch to [`require`](../globals.md) is when you paste the
code into the SuiteScript debugger to try it out; change it back before you deploy.

## Example: link a record to a Suitelet

A service team keeps an escalation timeline for each support case in a Suitelet. This user event
script adds a **Timeline** subtab to the case in view mode, with an inline HTML field that links to the
Suitelet and passes the case ID as a custom parameter (`custom_case`, because names such as `id` are
reserved on Suitelet URLs).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/ui/serverWidget', 'N/url'], (serverWidget, url) => {
  const beforeLoad = (context) => {
    if (context.type !== context.UserEventType.VIEW) {
      return;
    }

    const timelineUrl = url.resolveScript({
      scriptId: 'customscript_case_timeline',
      deploymentId: 'customdeploy_case_timeline',
      params: { custom_case: String(context.newRecord.id) }
    });

    context.form.addTab({ id: 'custpage_tab_timeline', label: 'Timeline' });
    const linkField = context.form.addField({
      id: 'custpage_timeline_link',
      type: serverWidget.FieldType.INLINEHTML,
      label: 'Escalation timeline',
      container: 'custpage_tab_timeline'
    });
    linkField.defaultValue = `<p><a href="${timelineUrl}" target="_blank">Open the escalation timeline for this case</a></p>`;
  };

  return { beforeLoad };
});
```

## Example: answer with XML

A desktop gadget polls this Suitelet for the current user's tasks. Instead of a page, the Suitelet
writes an XML document and sets the content type to match.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/search', 'N/runtime'], (search, runtime) => {
  const escapeXml = (text) => String(text === null || text === undefined ? '' : text)
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;');

  const onRequest = (context) => {
    const userId = runtime.getCurrentUser().id;
    const rows = [];

    search.create({
      type: search.Type.TASK,
      filters: [['assigned', 'anyof', userId]],
      columns: ['title', 'duedate', 'priority']
    }).run().each((result) => {
      const due = escapeXml(result.getValue({ name: 'duedate' }));
      const priority = escapeXml(result.getText({ name: 'priority' }));
      const title = escapeXml(result.getValue({ name: 'title' }));
      rows.push(`  <task ref="${result.id}" due="${due}" priority="${priority}">${title}</task>`);
      return rows.length < 50;
    });

    context.response.setHeader({ name: 'Content-Type', value: 'application/xml' });
    context.response.write({
      output: `<?xml version="1.0" encoding="UTF-8"?>\n<tasks owner="${userId}">\n${rows.join('\n')}\n</tasks>`
    });
  };

  return { onRequest };
});
```
