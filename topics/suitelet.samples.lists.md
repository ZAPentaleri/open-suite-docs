# Custom List Samples
<!-- id: topic:suitelet.samples.lists -->

Part of: [Suitelet Samples](suitelet.samples.md)

See also: [Suitelet](../script-types/Suitelet.md), [N/ui/serverWidget](../modules/N_ui_serverWidget.md),
[Custom fields](custom_fields.md), [globals: `define` and `require`](../globals.md).

The custom list group has two samples: a Suitelet that renders a stand-alone list page, and a
dashboard-link icon shown in a list column.

As with every entry-point script, write these with [`define`](../globals.md) for deployment and switch
to [`require`](../globals.md) only while testing in the debugger.

## The dashboard-icon column

The second sample puts a small "view dashboard" icon into lists. When users hover over a row in an
entity list, the icon appears and links to that entity's dashboard. Setting it up takes three steps:

1. Create a transaction body field (Customization, Transaction Body Fields, New) of type free-form
   text with **Store Value** cleared.
2. Give it a formula default value that produces an `<img>` tag for the icon.
3. Deploy the accompanying Suitelet with status **Released**.

Wrap the whole `<img>` default value in single quotes. Without the quotes, the formula is not valid and
saving the field fails with a formula error.

## Example: a list of my follow-up tasks

A sales rep wants a compact, printable list of the tasks assigned to them. The Suitelet builds a
stand-alone list in report style and fills it from a task search.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/search', 'N/runtime'], (serverWidget, search, runtime) => {
  const onRequest = (context) => {
    const me = runtime.getCurrentUser();
    const list = serverWidget.createList({ title: `Follow-ups for ${me.name}` });
    list.style = serverWidget.ListStyle.REPORT;

    list.addColumn({ id: 'subject', type: serverWidget.FieldType.TEXT, label: 'Task' });
    list.addColumn({ id: 'due', type: serverWidget.FieldType.TEXT, label: 'Due' });
    list.addColumn({ id: 'urgency', type: serverWidget.FieldType.TEXT, label: 'Priority' });
    list.addColumn({ id: 'state', type: serverWidget.FieldType.TEXT, label: 'Status' });

    const rows = [];
    search.create({
      type: search.Type.TASK,
      filters: [['assigned', 'anyof', me.id]],
      columns: ['title', 'duedate', 'priority', 'status']
    }).run().each((result) => {
      rows.push({
        subject: String(result.getValue({ name: 'title' }) || '-'),
        due: String(result.getValue({ name: 'duedate' }) || '-'),
        urgency: String(result.getText({ name: 'priority' }) || '-'),
        state: String(result.getText({ name: 'status' }) || '-')
      });
      return rows.length < 500;
    });

    list.addRows({ rows });
    context.response.writePage({ pageObject: list });
  };

  return { onRequest };
});
```
