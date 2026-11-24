# Portlet
<!-- id: script:Portlet -->

A portlet script builds a custom panel for a NetSuite dashboard. The code runs on the server and
produces the portlet's content; the result is drawn in the user's browser as part of the dashboard.
Depending on the portlet type you pick, the panel can be a small form, a block of free HTML, a list
of indented links, or a table of rows and columns.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `Portlet` |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Server (output is rendered in the client browser, inside the dashboard) |
| SuiteScript 1.0 counterpart | Portlet Scripts ([1.0 concepts](../topics/ss1_api.portlets.md)) |
| Entry points | [`render`](#rendercontext) |
| Portlet types | Simple Form, Inline HTML, Links and Indents, Simple List |

Related pages: [SDF object](../topics/sdf.portlet.md), [script deployment](../topics/script_deployment.md),
[script parameters](../topics/script_parameters.md), [N/portlet](../modules/N_portlet.md),
[N/ui/serverWidget](../modules/N_ui_serverWidget.md), [N/search](../modules/N_search.md).

## How it runs

**Setup before it appears anywhere.** A portlet has to be created and deployed before anyone can put
it on a dashboard. The steps are: upload the script file, create a script record with the portlet
type set, then create a deployment. The portlet type chosen on the script record must match what the
script builds.

**The four portlet types.**

| Type | What it shows | Members of the `Portlet` object you use |
|---|---|---|
| Simple Form | A small data-entry form. It supports refreshing and resizing the portlet with [`N/portlet`](../modules/N_portlet.md), and validation by a record-level client script | `addField`, `setSubmitButton`, `clientScriptFileId`, `clientScriptModulePath`, `title` |
| Inline HTML | Free-form HTML, which can include images and Flash | `html`, `title` |
| Links and Indents | Rows of formatted content | `addLine`, `title` |
| Simple List | A table whose columns and rows you define | `addColumn`, `addEditColumn`, `addRow`, `addRows`, `title` |

**Two ways onto a dashboard.** A portlet script can be offered as a dashboard SuiteApp portlet or as
a custom portlet:

- *Dashboard SuiteApp portlet.* Users add it from the SuiteApps tab of Personalize Dashboard. It is
  only available when a SuiteApp with a dashboard component is installed. This route lets you show a
  brand icon for the portlet in Personalize Dashboard. The script record has a dashboard SuiteApp
  setting that controls this; once the record has been saved with it, you cannot clear it again.
- *Custom portlet.* A user places a custom portlet on the dashboard and then selects your script in
  it.

**Icons for SuiteApp portlets.** The icon must be an SVG file; no other format is accepted. Design
guidance from the source: use the cloud motif as the background, stick to a restricted colour palette,
prefer geometric shapes over illustration, and give the shape a cast shadow running down and to the
right at 45 degrees, in black at 10 percent opacity. Save the result as SVG.

**Script parameters.** Do not delete script parameters from a portlet script; doing so can leave
published dashboards in an inconsistent state.

**Managing scripts.** Portlet scripts can be managed with SDF, copied between accounts with Copy to
Account, and reviewed with SuiteScript Analysis.

## Entry points

### render(context)
<!-- id: script:Portlet.render -->

The single entry point. NetSuite passes the context in automatically; your code fills in the
`Portlet` object, and what you build is what the dashboard displays. The context itself is read-only:
you work through the object it hands you rather than replacing its properties.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.portlet` | Portlet | - | |
| `context.column` | string | - | |
| `context.entity` | string | - | |

All three context properties exist since 2015.2.

- `context.portlet` is the [`Portlet`](#portlet-object) object to populate. Which of its members
  apply depends on the portlet type (see the table above).
- `context.column` is read-only and gives the dashboard column the portlet sits in, as a string
  index. The values the source lists are `left`, `center` and `right`.
- `context.entity` is read-only and holds the internal ID of the selected customer.

> [!SOURCE-ERROR] The source calls `column` a numeric column index held in a string and promises a
> list of the index values, but then lists only `left`, `center` and `right`. Log the value in your
> account before branching on it.
<!-- source-error: script:Portlet.render | column | column_numeric_index_values_promised_not_listed -->

**Returns:** `void`

**Errors:** the source does not state any errors for this entry point.

## Objects

### Portlet (object)
<!-- id: script:Portlet.Portlet -->

The object handed to `render` as `context.portlet`. Through it you define the portlet's columns,
fields, lines, rows and parameters, and optionally a submit button. Members are grouped by portlet
type: form members (`addField`, `setSubmitButton`, `clientScriptFileId`, `clientScriptModulePath`),
the Inline HTML member (`html`), the Links and Indents member (`addLine`) and the list members
(`addColumn`, `addEditColumn`, `addRow`, `addRows`). `title` works for every type. The client script
file ID property accepts either a number or a string.

#### Portlet.title
<!-- id: script:Portlet.Portlet#title -->

The portlet's title. It applies to all portlet types.

**Type:** `string`

**Since:** 2016.2

#### Portlet.html
<!-- id: script:Portlet.Portlet#html -->

The complete HTML content of an Inline HTML portlet. Only meaningful for that type.

**Type:** `string`

**Since:** 2016.2

#### Portlet.clientScriptModulePath
<!-- id: script:Portlet.Portlet#clientScriptModulePath -->

Path of a client script to attach to a form portlet. You can attach the client script either by path
(this property) or by file ID ([`clientScriptFileId`](#portletclientscriptfileid)), not both.

**Type:** `string`

**Errors:**

- `PROPERTY_VALUE_CONFLICT`: you set this property while `clientScriptFileId` is already set.

**Since:** 2016.2

#### Portlet.clientScriptFileId
<!-- id: script:Portlet.Portlet#clientScriptFileId -->

File ID of a client script to attach to a form portlet. Both a number and a string are accepted. This
is the alternative to [`clientScriptModulePath`](#portletclientscriptmodulepath); only one of the two
may be set.

**Type:** `number | string`

**Errors:**

- `PROPERTY_VALUE_CONFLICT`: you set this property while `clientScriptModulePath` is already set.

> [!SOURCE-ERROR] The source's error row for this property describes the conflict by pointing back at
> this same property instead of at the other client-script property. Read it as a clash with
> `clientScriptModulePath`, as stated above, and test it if your code depends on the exact behaviour.
<!-- source-error: script:Portlet.Portlet#clientScriptFileId | - | error_row_refers_to_own_page -->

**Since:** 2016.2

#### Portlet.addColumn(options)
<!-- id: script:Portlet.Portlet#addColumn -->

Adds a column to a Simple List portlet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.type` | string | Yes | |
| `options.align` | string | No | |
| `options.label` | string | Yes | |

- `options.id`: lowercase, without spaces.
- `options.type`: one of the [`serverWidget.FieldType`](../modules/N_ui_serverWidget.md) values.
- `options.align`: one of the [`serverWidget.LayoutJustification`](../modules/N_ui_serverWidget.md)
  values.

**Returns:** `serverWidget.ListColumn`

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2016.2

#### Portlet.addEditColumn(options)
<!-- id: script:Portlet.Portlet#addEditColumn -->

Adds an Edit (or Edit | View) link column to a Simple List portlet, placed to the left of an existing
column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.column` | string | Yes | |
| `options.showHrefCol` | string | No | |
| `options.showView` | boolean | No | `false` |
| `options.link` | string | No | |
| `options.linkParam` | string | No | |
| `options.linkParamName` | string | No | `id` |

- `options.column`: ID of the column that the edit column is added to the left of.
- `options.showHrefCol`: name of a column whose value decides, per row, whether the link is
  clickable. That column holds `T` or `F`.
- `options.showView`: `true` adds an Edit | View column; `false` (the default) adds an Edit column
  only.
- `options.link`: the link target. The final URL has the shape `link?linkParamName=<linkParam value
  for the row>`; the source's illustration is the employee entity page with `id=123` appended.
  Since 2019.2.
- `options.linkParam`: which row value is appended to the link. When omitted, the value of
  `options.column` is used. In most cases `internalid` is the recommended value. Since 2019.2.
- `options.linkParamName`: the query-string parameter name; defaults to `id`. Since 2019.2.

> [!SOURCE-ERROR] The source describes `showHrefCol` as a column name, but its syntax sample passes the
> boolean `true` instead. Pass a column name as described and verify the behaviour in your account.
<!-- source-error: script:Portlet.Portlet#addEditColumn | options.showHrefCol | syntax_sample_passes_boolean_true_not_column_name -->

**Returns:** `serverWidget.ListColumn`

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2016.2 (the `link`, `linkParam` and `linkParamName` options since 2019.2)

#### Portlet.addRow(options)
<!-- id: script:Portlet.Portlet#addRow -->

Adds one row to a Simple List portlet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.row` | search.Result \| object | Yes | |

- `options.row`: either a search result, or a plain object of name/value pairs whose names match the
  column IDs you added.

**Returns:** `Object`. The source does not describe what this object is or contains.

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2016.2

#### Portlet.addRows(options)
<!-- id: script:Portlet.Portlet#addRows -->

Adds several rows to a Simple List portlet in one call.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.rows` | search.Result[] \| object[] | Yes | |

- `options.rows`: an array of search results, or an array of name/value objects whose names match
  the column IDs.

**Returns:** `Object`. The source does not describe what this object is or contains.

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2016.2

#### Portlet.addLine(options)
<!-- id: script:Portlet.Portlet#addLine -->

Adds a line of text, optionally a link, to a Links and Indents portlet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.align` | number | No | |
| `options.text` | string | Yes | |
| `options.url` | string | No | |

- `options.align`: how far to indent the line, as a number of spaces from 0 to 5.
- `options.text`: the text shown.
- `options.url`: the address the line links to.

**Returns:** `Object`. The source does not describe what this object is or contains.

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2016.2

#### Portlet.addField(options)
<!-- id: script:Portlet.Portlet#addField -->

Adds a field to a Simple Form portlet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.type` | string | Yes | |
| `options.source` | string | No | |
| `options.label` | string | Yes | |

- `options.id`: lowercase, without spaces. When the field is added to an existing page it must start
  with `custpage`, for example `custpage_purchase_details`.
- `options.type`: one of the [`serverWidget.FieldType`](../modules/N_ui_serverWidget.md) values.
- `options.source`: only for select and multi-select fields. Give the internal ID or script ID of the
  source list. For radio fields, put the field's internal ID in the source. Once a source is set you
  cannot add options with [`Field.addSelectOption`](../modules/N_ui_serverWidget.md).

**Returns:** `serverWidget.Field`

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2016.2

#### Portlet.setSubmitButton(options)
<!-- id: script:Portlet.Portlet#setSubmitButton -->

Adds a submit button to a Simple Form portlet and says where the form posts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.url` | string | Yes | |
| `options.target` | string | No | |
| `options.label` | string | No | |

- `options.url`: the URL the form posts to.
- `options.target`: where the response is shown. Without it, the response loads in the portlet's own
  embedded iframe. Accepted values are `_top`, `_parent`, `_blank`, the name of a frame, or
  `_hidden`; with `_hidden`, the results go to a hidden child iframe.
- `options.label`: the button's label.

**Returns:** `serverWidget.Button`

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2016.2

## Examples

A Simple List portlet for managers: their direct reports, with an Edit | View link on each row that
opens the employee record. The rows are plain objects keyed by column ID. The script record's portlet
type must be Simple List.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Portlet
 */
define(['N/search', 'N/runtime', 'N/ui/serverWidget'], (search, runtime, serverWidget) => {
  const COLUMNS = ['internalid', 'entityid', 'title', 'phone'];

  const render = (context) => {
    const panel = context.portlet;
    panel.title = 'My direct reports';

    panel.addColumn({ id: 'internalid', type: serverWidget.FieldType.INTEGER, label: 'ID' });
    panel.addColumn({ id: 'entityid', type: serverWidget.FieldType.TEXT, label: 'Name' });
    panel.addColumn({ id: 'title', type: serverWidget.FieldType.TEXT, label: 'Job title' });
    panel.addColumn({
      id: 'phone',
      type: serverWidget.FieldType.TEXT,
      label: 'Phone',
      align: serverWidget.LayoutJustification.RIGHT
    });
    panel.addEditColumn({
      column: 'entityid',
      showView: true,
      link: '/app/common/entity/employee.nl',
      linkParam: 'internalid',
      linkParamName: 'id'
    });

    const results = search.create({
      type: search.Type.EMPLOYEE,
      filters: [['supervisor', 'anyof', runtime.getCurrentUser().id]],
      columns: COLUMNS
    }).run().getRange({ start: 0, end: 30 });

    panel.addRows({
      rows: results.map((result) => {
        const row = {};
        COLUMNS.forEach((name) => {
          row[name] = String(result.getValue({ name }) || '');
        });
        return row;
      })
    });
  };

  return { render };
});
```

A Simple Form portlet for a support desk: log a callback request without leaving the dashboard. The
form posts to a Suitelet whose URL is resolved at render time; `_hidden` sends the results to a hidden
child iframe. A client script is attached by path, so `clientScriptFileId` must stay unset.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Portlet
 */
define(['N/ui/serverWidget', 'N/url'], (serverWidget, url) => {
  const render = (context) => {
    const panel = context.portlet;
    panel.title = 'Request a callback';
    panel.clientScriptModulePath = './callback_portlet_cs.js';

    panel.addField({ id: 'custpage_case_number', type: serverWidget.FieldType.TEXT, label: 'Case number' });
    panel.addField({ id: 'custpage_callback_on', type: serverWidget.FieldType.DATE, label: 'Call back on' });
    panel.addField({ id: 'custpage_callback_note', type: serverWidget.FieldType.TEXTAREA, label: 'What to discuss' });

    panel.setSubmitButton({
      url: url.resolveScript({
        scriptId: 'customscript_callback_intake',
        deploymentId: 'customdeploy_callback_intake'
      }),
      target: '_hidden',
      label: 'Queue callback'
    });
  };

  return { render };
});
```

An Inline HTML portlet that shows a short notice. When the dashboard belongs to a selected customer,
`context.entity` holds that customer's internal ID and the notice mentions it; the script makes no
assumption about the format of `context.column`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Portlet
 */
define([], () => {
  const escapeHtml = (text) => String(text)
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;');

  const render = (context) => {
    const panel = context.portlet;
    panel.title = 'Warehouse notice';
    const who = context.entity ? `customer ${escapeHtml(context.entity)}` : 'all customers';
    panel.html = [
      '<div style="padding:8px;font-size:13px">',
      `<p><strong>Dock 3 closed for maintenance.</strong> Pickups for ${who} move to Dock 1.</p>`,
      '</div>'
    ].join('');
  };

  return { render };
});
```
