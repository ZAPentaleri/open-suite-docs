# Assistants, lists, portlets and template rendering (SuiteScript 1.0)

> **Legacy API.** This page describes SuiteScript 1.0 UI objects so that tools can read, convert and
> minimally repair existing 1.0 scripts. It is not a guide for new development. New code belongs in
> SuiteScript 2.1: [N/ui/serverWidget](../modules/N_ui_serverWidget.md) for assistants and lists, the
> [Portlet script type](../script-types/Portlet.md) for portlets, and
> [N/render](../modules/N_render.md) for template rendering. The
> [1.0 to 2.1 conversion guide](../conversion/ss1-to-ss21.md) covers migration.

Related pages: [1.0 Suitelets](../topics/ss1_api.suitelets.md),
[1.0 portlet scripts](../topics/ss1_api.portlets.md),
[1.0 client scripts](../topics/ss1_api.client_scripts.md),
[assistants built from UI objects](../topics/ui_assistants.md),
[forms, fields, sublists and columns in 1.0](objects-forms.md).

How to read this page:

- 1.0 methods take **positional** arguments. Parameter tables list them in call order; keep that
  order when repairing or converting a call.
- Line and step numbers start at 1.
- Every method on this page has a documented cost of 0 usage units.
- The source gives no release ("since") for the objects themselves; each method shows the
  **Since:** release the source gives for it.
- `nlobjField`, `nlobjFieldGroup`, `nlobjSubList`, `nlobjButton` and `nlobjColumn`, which several
  methods here return, are documented on [the forms page](objects-forms.md).

## Objects

### nlobjAssistant (object)
<!-- id: ss1:nlobjAssistant -->

A multi-step wizard page built in a Suitelet, obtained from `nlapiCreateAssistant`. The assistant
keeps its state per user session until it is finished, so on every request the Suitelet can tell
which step the user came from and what they did there.

> [!SOURCE-ERROR] The source's reference for `nlobjResponse.writePage` lists only forms and lists as
> what it accepts, yet its assistant examples pass an `nlobjAssistant` to it. Writing an assistant
> with `writePage` is what the examples show; the reference text does not confirm it.
<!-- source-error: ss1:nlobjAssistant | - | writepage_ref_lists_form_list_examples_pass_assistant -->

#### nlobjAssistant.addField(name, type, label, source, group)
<!-- id: ss1:nlobjAssistant#addField -->

Adds a field to the assistant.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | No | |
| `source` | number \| string | No | |
| `group` | string | No | |

- `type`: one of `text`, `email`, `radio`, `label`, `phone`, `date`, `currency`, `float`,
  `integer`, `checkbox`, `select`, `url`, `timeofday`, `textarea`, `multiselect`, `image`,
  `inlinehtml`, `password`, `help`, `percent`; `longtext` and `richtext` are also accepted.
- `source`: where a select field's options come from. A sourced select takes its values from that
  source, so you cannot add options to it; pass null as the source when you want to supply the
  options yourself with `nlobjField.addSelectOption`.
- `group`: the field group to place the field in.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.addFieldGroup(name, label)
<!-- id: ss1:nlobjAssistant#addFieldGroup -->

Adds a field group. Assistant field groups start out collapsible and expanded; change that with
`nlobjFieldGroup.setCollapsible`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `label` | string | Yes | |

**Returns:** `nlobjFieldGroup`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.addStep(name, label)
<!-- id: ss1:nlobjAssistant#addStep -->

Adds a step. Unless configured otherwise, the steps are listed vertically in a panel on the left.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `label` | string | Yes | |

**Returns:** `nlobjAssistantStep`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.addSubList(name, type, label)
<!-- id: ss1:nlobjAssistant#addSubList -->

Adds a sublist to the assistant.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | Yes | |

- `type`: the only accepted value is `inlineeditor`.

**Returns:** `nlobjSubList` (see [objects-forms.md](objects-forms.md)).

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getAllFields()
<!-- id: ss1:nlobjAssistant#getAllFields -->

Returns the names of the fields across all steps. Only fields added before the call are included.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getAllFieldGroups()
<!-- id: ss1:nlobjAssistant#getAllFieldGroups -->

Returns the names of the field groups. Only groups added before the call are included.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getAllSubLists()
<!-- id: ss1:nlobjAssistant#getAllSubLists -->

Returns the names of the sublists. Only sublists added before the call are included.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getCurrentStep()
<!-- id: ss1:nlobjAssistant#getCurrentStep -->

Returns the step most recently made current with `setCurrentStep`.

**Returns:** `nlobjAssistantStep`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getLastAction()
<!-- id: ss1:nlobjAssistant#getLastAction -->

Tells you what the user did on the page they just submitted: `next`, `back`, `cancel`, `finish` or
`jump`. `jump` means the user picked a step directly, which is possible when steps are unordered.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getLastStep()
<!-- id: ss1:nlobjAssistant#getLastStep -->

Returns the step on which the last submitted action happened, that is, the step the user came from.

**Returns:** `nlobjAssistantStep`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getNextStep()
<!-- id: ss1:nlobjAssistant#getNextStep -->

Returns the step that should come next. Call it only after you have captured the data of the last
step; its usual use is to feed the result to `setCurrentStep`.

**Returns:** `nlobjAssistantStep`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.hasError()
<!-- id: ss1:nlobjAssistant#hasError -->

Returns `true` when `setError` has been called.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.isFinished()
<!-- id: ss1:nlobjAssistant#isFinished -->

Returns `true` when all steps are done or `setFinished` has been called.

**Returns:** `boolean`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.sendRedirect(response)
<!-- id: ss1:nlobjAssistant#sendRedirect -->

Redirects the assistant to itself, which is how a POST handler moves on to the next page. When the
user cancels or finishes and the assistant was opened from another assistant, control returns to
the calling one.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `response` | nlobjResponse | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setError(html)
<!-- id: ss1:nlobjAssistant#setError -->

Sets an error message for the current page; plain text or HTML is accepted.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `html` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setFinished(html)
<!-- id: ss1:nlobjAssistant#setFinished -->

Marks the assistant as finished and shows the given completion message on its last page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `html` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setNumbered(hasStepNumber)
<!-- id: ss1:nlobjAssistant#setNumbered -->

Turns step numbering on or off.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `hasStepNumber` | boolean | No | |

- `hasStepNumber`: `false` shows the steps without numbers.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2010.1

#### nlobjAssistant.setOrdered(ordered)
<!-- id: ss1:nlobjAssistant#setOrdered -->

Chooses whether the steps must be completed in sequence. An assistant is ordered unless you set it
otherwise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `ordered` | boolean | Yes | |

- `ordered`: `true` makes the steps sequential, listed vertically; `false` lets the user complete
  them in any order, listed horizontally.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setScript(script)
<!-- id: ss1:nlobjAssistant#setScript -->

Attaches a global client script to the assistant.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `script` | string \| number | Yes | |

- `script`: the script ID or internal ID of a global client script.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setShortcut(show)
<!-- id: ss1:nlobjAssistant#setShortcut -->

Shows or hides the "add to shortcuts" link. The link is shown unless you hide it, and it is always
hidden on external pages.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `show` | boolean | Yes | |

- `show`: `false` hides the link.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setSplash(title, text1, text2)
<!-- id: ss1:nlobjAssistant#setSplash -->

Sets the splash screen shown when the assistant starts.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |
| `text1` | string | Yes | |
| `text2` | string | No | |

- `text2`: when given, the splash screen switches to a two-column layout.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setTitle(title)
<!-- id: ss1:nlobjAssistant#setTitle -->

Sets the assistant's title, overriding the one passed to `nlapiCreateAssistant`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getAllSteps()
<!-- id: ss1:nlobjAssistant#getAllSteps -->

Returns all steps.

**Returns:** `nlobjAssistantStep[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getField(name)
<!-- id: ss1:nlobjAssistant#getField -->

Looks up a field by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjField`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getFieldGroup(name)
<!-- id: ss1:nlobjAssistant#getFieldGroup -->

Looks up a field group by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjFieldGroup`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getStep(name)
<!-- id: ss1:nlobjAssistant#getStep -->

Looks up a step by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjAssistantStep`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getSubList(name)
<!-- id: ss1:nlobjAssistant#getSubList -->

Looks up a sublist by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjSubList`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setCurrentStep(step)
<!-- id: ss1:nlobjAssistant#setCurrentStep -->

Makes the given step the current one.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `step` | nlobjAssistantStep | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.setFieldValues(values)
<!-- id: ss1:nlobjAssistant#setFieldValues -->

Sets several field values at once from an object that maps field names to values.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `values` | Object | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistant.getStepCount()
<!-- id: ss1:nlobjAssistant#getStepCount -->

Returns the number of steps.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2009.2

### nlobjAssistantStep (object)
<!-- id: ss1:nlobjAssistantStep -->

One step of an `nlobjAssistant`, returned by `addStep`, `getStep`, `getCurrentStep`, `getLastStep`
and `getNextStep`. Its getters read what the user entered on that step.

#### nlobjAssistantStep.getAllFields()
<!-- id: ss1:nlobjAssistantStep#getAllFields -->

Returns the names of the fields the user filled in on this step.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.getFieldValues(name)
<!-- id: ss1:nlobjAssistantStep#getFieldValues -->

Reads the selected values of a multiselect field on this step. The returned array is read-only.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string[]`, or null when the field does not exist.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.getLineItemCount(group)
<!-- id: ss1:nlobjAssistantStep#getLineItemCount -->

Returns the number of lines the user entered in a sublist of this step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |

**Returns:** `number`; -1 when the sublist does not exist.

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.getLineItemValue(group, name, linenum)
<!-- id: ss1:nlobjAssistantStep#getLineItemValue -->

Reads one sublist value the user entered on this step.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjAssistantStep#getLineItemValue | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `name` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: 1-based line number.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.getStepNumber()
<!-- id: ss1:nlobjAssistantStep#getStepNumber -->

Returns the step's position; the first step is 1.

**Returns:** `number`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.getAllLineItemFields(group)
<!-- id: ss1:nlobjAssistantStep#getAllLineItemFields -->

Returns the field names of a sublist on this step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.getAllLineItems()
<!-- id: ss1:nlobjAssistantStep#getAllLineItems -->

Returns the names of the sublists on this step.

**Returns:** `string[]`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.getFieldValue(name)
<!-- id: ss1:nlobjAssistantStep#getFieldValue -->

Reads the value the user entered in a field on this step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.setHelpText(help)
<!-- id: ss1:nlobjAssistantStep#setHelpText -->

Sets help text for the step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `help` | string | Yes | |

**Returns:** `nlobjAssistantStep`

> [!SOURCE-ERROR] The source's return-type cell for this method is cut off part-way through the
> object name. The spec reads it as `nlobjAssistantStep`, which matches `setLabel` on the same
> object; the source itself does not show the full name.
<!-- source-error: ss1:nlobjAssistantStep#setHelpText | - | returns_cell_truncated_nlobjassistantste -->

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjAssistantStep.setLabel(label)
<!-- id: ss1:nlobjAssistantStep#setLabel -->

Changes the step's label.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | Yes | |

**Returns:** `nlobjAssistantStep`

**Governance:** 0 units.

**Since:** 2009.2

### nlobjList (object)
<!-- id: ss1:nlobjList -->

A standalone list page that a Suitelet builds with `nlapiCreateList` and writes to the response.

#### nlobjList.addButton(name, type, script)
<!-- id: ss1:nlobjList#addButton -->

Adds a button to the page footer.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjList#addButton | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `script` | string | No | |

- `name`: lowercase, no spaces.
- `type`: despite its name, this is the label shown on the button in the UI.
- `script`: the name of the function to run on click.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjList.addColumn(name, type, label, align)
<!-- id: ss1:nlobjList#addColumn -->

Adds a column to the list.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | Yes | |
| `align` | string | No | |

- `type`: one of `text`, `email`, `phone`, `date`, `currency`, `float`, `integer`, `select`, `url`,
  `timeofday`, `textarea`, `percent`, `inlinehtml`.
- `align`: `center`, `right` or `left`; `left` when omitted.

**Returns:** `nlobjColumn` (see [objects-forms.md](objects-forms.md)).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjList.addEditColumn(column, showView, showHrefCol)
<!-- id: ss1:nlobjList#addEditColumn -->

Adds an Edit link column immediately to the left of the given column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `column` | nlobjColumn | Yes | |
| `showView` | boolean | No | |
| `showHrefCol` | boolean | No | |

- `showView`: `true` shows both Edit and View links.
- `showHrefCol`: names a column that must be present in the row data; its value (`T` or `F`) decides
  whether each row's link is clickable.

> [!SOURCE-ERROR] `showHrefCol` is typed boolean here but string on the portlet method of the same
> name. Given that its values are `T`/`F`, check how an existing script passes it.
<!-- source-error: ss1:nlobjList#addEditColumn | showHrefCol | type_boolean_here_string_on_nlobjportlet -->

**Returns:** `nlobjColumn`

**Governance:** 0 units.

**Since:** 2008.1

#### nlobjList.addPageLink(type, title, url)
<!-- id: ss1:nlobjList#addPageLink -->

Adds a navigation link at the top of the list page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `title` | string | Yes | |
| `url` | string | Yes | |

- `type`: `breadcrumb` places the link at the top left, after the system breadcrumbs; `crosslink`
  places it at the top right.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjList.addRow(row)
<!-- id: ss1:nlobjList#addRow -->

Adds one row from a plain object or a search result. A search-result row also carries the record
type (`recordtype`), the internal ID (`id`) and, for each field, a `<fieldname>_display` value with
its display text.

> [!SOURCE-ERROR] The source's description for this list method talks about a portlet, apparently
> copied from `nlobjPortlet.addRow`. It applies to the list.
<!-- source-error: ss1:nlobjList#addRow | - | description_says_portlet -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `row` | Object \| nlobjSearchResult | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjList.addRows(rows)
<!-- id: ss1:nlobjList#addRows -->

Adds several rows from plain objects or search results. Search-result rows also carry `recordtype`,
`id` and `<fieldname>_display` values.

> [!SOURCE-ERROR] The source's description for this list method talks about a portlet, apparently
> copied from `nlobjPortlet.addRows`. It applies to the list.
<!-- source-error: ss1:nlobjList#addRows | - | description_says_portlet -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `rows` | Object[] \| nlobjSearchResult[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjList.setScript(script)
<!-- id: ss1:nlobjList#setScript -->

Attaches a global client script to the list page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `script` | string \| number | Yes | |

- `script`: the script ID or internal ID of a global client script.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjList.setStyle(style)
<!-- id: ss1:nlobjList#setStyle -->

Sets the visual style of the list.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `style` | string | Yes | |

- `style`: `grid`, `report`, `plain` or `normal`.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjList.setTitle(title)
<!-- id: ss1:nlobjList#setTitle -->

Sets the list page's title.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

### nlobjPortlet (object)
<!-- id: ss1:nlobjPortlet -->

The portlet a 1.0 portlet script fills in. It arrives as the first argument of the portlet function;
an optional second argument gives the dashboard column the portlet sits in (1 = left, 2 = middle,
3 = right). A portlet is one of four kinds, `LIST`, `FORM`, `HTML` or `LINKS`, and most methods below
work for only one kind.

#### nlobjPortlet.addColumn(name, type, label, just)
<!-- id: ss1:nlobjPortlet#addColumn -->

Adds a column to a list portlet. It returns an `nlobjColumn`, the same column object that
`nlobjList.addColumn` returns, so `setURL`, `addParamToURL` and `setLabel` (see
[objects-forms.md](objects-forms.md)) can be used on it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | Yes | |
| `just` | string | No | |

- `type`: one of `text`, `email`, `phone`, `date`, `currency`, `float`, `integer`, `select`, `url`,
  `timeofday`, `textarea`, `percent`, `inlinehtml`; `checkbox` is also accepted.
- `just`: justification, `center`, `right` or `left`; `left` when omitted.

**Returns:** `nlobjColumn`

**Governance:** 0 units.

**Availability:** list portlets only.

**Since:** 2008.2

#### nlobjPortlet.addEditColumn(column, showView, showHrefCol)
<!-- id: ss1:nlobjPortlet#addEditColumn -->

Adds an Edit link column immediately to the left of the given column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `column` | nlobjColumn | Yes | |
| `showView` | boolean | No | |
| `showHrefCol` | string | No | |

- `showView`: the source gives no further detail for the portlet version (on `nlobjList` it adds a
  View link next to Edit).
- `showHrefCol`: names a column that must be present in the row data; its value (`T` or `F`) decides
  whether each row's link is clickable. Typed string here, boolean on `nlobjList.addEditColumn`.

**Returns:** `nlobjColumn`

**Governance:** 0 units.

**Availability:** list portlets only.

**Since:** 2008.1

#### nlobjPortlet.addField(name, type, label, source)
<!-- id: ss1:nlobjPortlet#addField -->

Adds a field to a form portlet. A select field with a source takes its options from it, so you
cannot add options to it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | Yes | |
| `source` | number \| string | No | |

- `type`: one of `text`, `email`, `phone`, `date`, `currency`, `float`, `integer`, `select`, `url`,
  `timeofday`, `textarea`, `percent`, `inlinehtml`; `checkbox` is also accepted.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Availability:** form portlets only.

**Since:** 2008.2

#### nlobjPortlet.addLine(text, url, indent)
<!-- id: ss1:nlobjPortlet#addLine -->

Adds a line to a links portlet. The text may be plain or basic HTML.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `text` | string | Yes | |
| `url` | string | No | |
| `indent` | number | No | |

- `url`: the link target; null makes the line plain, not clickable.
- `indent`: indentation level from 0 to 5.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** links portlets only.

**Since:** 2008.2

#### nlobjPortlet.addRow(row)
<!-- id: ss1:nlobjPortlet#addRow -->

Adds one row to a list portlet. A search-result row also carries `recordtype`, `id` and
`<fieldname>_display` values.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `row` | Object \| nlobjSearchResult | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** list portlets only.

**Since:** 2008.2

#### nlobjPortlet.addRows(rows)
<!-- id: ss1:nlobjPortlet#addRows -->

Adds several rows to a list portlet. Each row must have a value for every column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `rows` | Object[] \| nlobjSearchResult[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** list portlets only.

**Since:** 2008.2

#### nlobjPortlet.setHtml(html)
<!-- id: ss1:nlobjPortlet#setHtml -->

Sets the content of an HTML portlet. The content ends up inside a table cell, so wrap it in a `div`,
`table` or `span`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `html` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Availability:** HTML portlets only.

**Since:** 2008.2

#### nlobjPortlet.setRefreshInterval(n)
<!-- id: ss1:nlobjPortlet#setRefreshInterval -->

Makes a form portlet refresh itself every `n` seconds.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `n` | number | Yes | |

- `n`: seconds; in production the minimum is 60.

**Returns:** `void`

**Errors:**

- An error is raised when `n` is negative, or below 60 in production. The source does not give the
  error code.

**Governance:** 0 units.

**Availability:** form portlets only.

**Since:** 2011.1

#### nlobjPortlet.setScript(scriptid)
<!-- id: ss1:nlobjPortlet#setScript -->

Attaches a client script to a form portlet. Only one script can be attached; a second call replaces
the first.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `scriptid` | number \| string | Yes | |

- `scriptid`: the ID of a record-level client script.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** form portlets only.

**Since:** 2011.1

#### nlobjPortlet.setSubmitButton(url, label, target)
<!-- id: ss1:nlobjPortlet#setSubmitButton -->

Adds the submit button of a form portlet; submitting posts the form to `url`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `url` | string | Yes | |
| `label` | string | No | |
| `target` | string | No | |

- `url`: where the form is posted.
- `label`: "Save" when omitted.
- `target`: `_top`, `_parent`, `_blank`, a frame name, or `_hidden`, which loads the result into a
  hidden child iframe. This parameter was added in 2011.1.

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Availability:** form portlets only.

**Since:** 2008.2 (the `target` parameter since 2011.1).

#### nlobjPortlet.setTitle(title)
<!-- id: ss1:nlobjPortlet#setTitle -->

Sets the portlet's title.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

### nlobjTemplateRenderer (object)
<!-- id: ss1:nlobjTemplateRenderer -->

Renders FreeMarker templates against record or search data; obtain one from
`nlapiCreateTemplateRenderer`. It exists for transaction printing, printing of non-transaction
records, and bulk printing. Calls go in a fixed order: `setTemplate` first, then `addRecord` or
`addSearchResults`, then `renderToString` or `renderToResponse`.

**Requires feature:** `Advanced PDF/HTML Templates` must be enabled in the account.

#### nlobjTemplateRenderer.renderToString()
<!-- id: ss1:nlobjTemplateRenderer#renderToString -->

Renders the template and returns the result as an XML string, which is meant to be handed to
`nlapiXMLToPDF`.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjTemplateRenderer.setTemplate(template)
<!-- id: ss1:nlobjTemplateRenderer#setTemplate -->

Sets the FreeMarker template source.

> [!SOURCE-ERROR] The source's API map row shows this method without its `template` argument. The
> method's own entry has the parameter below; keep passing it.
<!-- source-error: ss1:nlobjTemplateRenderer#setTemplate | - | api_map_row_signature_omits_template_param -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `template` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjTemplateRenderer.renderToResponse()
<!-- id: ss1:nlobjTemplateRenderer#renderToResponse -->

Renders the template and writes the output to the response.

> [!SOURCE-ERROR] The source lists no parameters for this method, but its examples pass the
> `nlobjResponse` to it. The spec records no parameters and no return type; when repairing a call,
> keep the response argument an existing script already passes.
<!-- source-error: ss1:nlobjTemplateRenderer#renderToResponse | - | rendertoresponse_params_none_but_examples_pass_response -->

**Returns:** not stated in the source; the rendered output goes to the response.

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjTemplateRenderer.addRecord(var, record)
<!-- id: ss1:nlobjTemplateRenderer#addRecord -->

Makes a record available to the template under the variable name `var`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `var` | string | Yes | |
| `record` | nlobjRecord | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

#### nlobjTemplateRenderer.addSearchResults(var, searchResult)
<!-- id: ss1:nlobjTemplateRenderer#addSearchResults -->

Makes search results available to the template under the variable name `var`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `var` | string | Yes | |
| `searchResult` | nlobjSearchResult | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2013.1

## Examples

A list portlet showing recent support cases. The search results go straight into `addRows`, and the
column names match the search columns so each cell finds its value.

```js
function recentCasesPortlet(portlet, column) {
  portlet.setTitle(column === 2 ? 'Recent cases' : 'Cases');

  portlet.addColumn('casenumber', 'text', 'Case');
  portlet.addColumn('title', 'text', 'Subject');
  portlet.addColumn('startdate', 'date', 'Opened', 'right');

  var results = nlapiSearchRecord('supportcase', null, null, [
    new nlobjSearchColumn('casenumber'),
    new nlobjSearchColumn('title'),
    new nlobjSearchColumn('startdate')
  ]);

  if (results) {
    portlet.addRows(results.slice(0, 20));
  }
}
```

A helper that builds a Suitelet list page of project tasks, where the task name links to a detail
page and passes each row's internal ID. The Suitelet hands the returned list to
`response.writePage`. The `id` value is the internal ID that search-result rows carry automatically.

```js
function buildProjectTaskList(detailUrl) {
  var list = nlapiCreateList('Open project tasks');
  list.setStyle('report');

  var subject = list.addColumn('title', 'text', 'Task');
  subject.setURL(detailUrl);
  subject.addParamToURL('custparam_task', 'id', true);

  list.addColumn('startdate', 'date', 'Start');
  list.addColumn('estimatedwork', 'float', 'Estimate (h)', 'right');

  var results = nlapiSearchRecord('projecttask', null, null, [
    new nlobjSearchColumn('title'),
    new nlobjSearchColumn('startdate'),
    new nlobjSearchColumn('estimatedwork')
  ]);
  if (results) {
    list.addRows(results);
  }
  return list;
}
```

The POST half of an assistant Suitelet: inspect what the user did, validate the step they came from,
then either stay put with an error, advance, or finish. Every branch ends with `sendRedirect`, which
redirects the assistant to itself.

```js
function handleEscalationPost(request, response, assistant) {
  var action = assistant.getLastAction();
  if (action === 'cancel') {
    assistant.sendRedirect(response);
    return;
  }

  var fromStep = assistant.getLastStep();
  if (fromStep.getStepNumber() === 1) {
    var hours = parseFloat(fromStep.getFieldValue('custpage_hours_lost'));
    if (isNaN(hours) || hours < 0) {
      assistant.setError('Enter the hours lost as a number of zero or more.');
      assistant.sendRedirect(response);
      return;
    }
  }

  if (action === 'finish') {
    assistant.setFinished('The escalation request has been recorded.');
  } else {
    assistant.setCurrentStep(assistant.getNextStep());
  }
  assistant.sendRedirect(response);
}
```

A scheduled-script helper that prints one purchase order through a FreeMarker template and files the
PDF in the File Cabinet. The calls follow the required order: template, then data, then render. The
account needs the `Advanced PDF/HTML Templates` feature.

```js
function archivePurchaseOrderPdf(poId, folderId) {
  var po = nlapiLoadRecord('purchaseorder', poId);

  var template = '<?xml version="1.0"?>\n' +
    '<pdf><head></head><body>' +
    '<h1>Purchase order ${record.tranid}</h1>' +
    '<p>Due ${record.duedate}</p>' +
    '</body></pdf>';

  var renderer = nlapiCreateTemplateRenderer();
  renderer.setTemplate(template);
  renderer.addRecord('record', po);

  var pdf = nlapiXMLToPDF(renderer.renderToString());
  pdf.setFolder(folderId);
  return nlapiSubmitFile(pdf);
}
```
