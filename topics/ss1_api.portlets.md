# SuiteScript 1.0 portlet scripts
<!-- id: topic:ss1_api.portlets -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is [Portlet](../script-types/Portlet.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## When it runs

A portlet script runs each time the dashboard that holds the portlet is opened, and each time the
user refreshes that portlet.

## Entry function arguments

| Position | Argument | What it holds |
|---|---|---|
| 1 | `portlet` | The [`nlobjPortlet`](../ss1/objects-ui-other.md) to fill in |
| 2 | `column` | Where the portlet sits: 1 is the left column, 2 the middle, 3 the right |
| 3 | `entityid` | The customer's ID; only passed on customer dashboards |

## Converting

In 2.1 the function becomes the `render` entry point of [Portlet](../script-types/Portlet.md) and
the first two arguments arrive as `params.portlet` and `params.column`. See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

A links portlet that lists high-priority support cases and indents the links by one level only
when the portlet sits in the middle column. `addLine` takes the text, an optional URL and an indent
from 0 to 5.

```js
function urgentCasesPortlet(portlet, column) {
  portlet.setTitle('Urgent cases');
  var filters = [new nlobjSearchFilter('priority', null, 'anyof', '1')];
  var columns = [new nlobjSearchColumn('title')];
  var rows = nlapiSearchRecord('supportcase', null, filters, columns) || [];
  var indent = column === 2 ? 1 : 0;
  for (var i = 0; i < rows.length && i < 10; i++) {
    var url = nlapiResolveURL('RECORD', 'supportcase', rows[i].getId());
    portlet.addLine(rows[i].getValue('title'), url, indent);
  }
}
```
