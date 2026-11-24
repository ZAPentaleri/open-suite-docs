# N/portlet
<!-- id: N/portlet -->

`N/portlet` gives client-side code two small controls over a form portlet on the dashboard: make it
re-measure its size, or reload it. Both take effect immediately. It is used together with
[Portlet scripts](../script-types/Portlet.md), typically from the client script attached to a
portlet's form.

- **Availability:** client scripts only.
- **Load it as:** `define(['N/portlet'], (portlet) => { ... })`
- **Since:** 2016.1

## Members

| Member | Kind | Summary |
|---|---|---|
| [`portlet.resize()`](#portletresize) | function | Resize the form portlet now |
| [`portlet.refresh()`](#portletrefresh) | function | Refresh the form portlet now |

## Functions

### portlet.resize()
<!-- id: N/portlet.resize -->

Resizes the form portlet straight away, for example after your code has shown or hidden content so
that the portlet's frame fits again. Takes no parameters.

**Returns:** `Void`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

### portlet.refresh()
<!-- id: N/portlet.refresh -->

Reloads the form portlet straight away. Takes no parameters.

**Returns:** `Void`

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

## Examples

A client script for a quick-note form portlet. Ticking a checkbox fills a long text field with a
multi-line template, which makes the form taller, so the portlet is resized to fit. A second helper
reloads the portlet, for example after the note has been submitted.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/portlet'], (portlet) => {
  const TEMPLATE = ['Caller:', 'Topic:', 'Promised follow-up:', 'Owner:'].join('\n');

  const pageInit = () => {};

  const fieldChanged = (context) => {
    if (context.fieldId !== 'custpage_use_template') {
      return;
    }
    const useTemplate = context.currentRecord.getValue({ fieldId: 'custpage_use_template' });
    context.currentRecord.setValue({
      fieldId: 'custpage_note_body',
      value: useTemplate ? TEMPLATE : '',
      ignoreFieldChange: true
    });
    portlet.resize();
  };

  const reloadPortlet = () => {
    portlet.refresh();
  };

  return { pageInit, fieldChanged, reloadPortlet };
});
```
