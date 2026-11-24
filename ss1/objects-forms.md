# Forms, fields and sublists (SuiteScript 1.0)

> **Legacy API.** This page describes the SuiteScript 1.0 form-building objects so that tools can
> read, convert and minimally repair existing 1.0 scripts. It is not a guide for new development:
> new code belongs in SuiteScript 2.1 with [N/ui/serverWidget](../modules/N_ui_serverWidget.md), and
> the [1.0 to 2.1 conversion guide](../conversion/ss1-to-ss21.md) covers migration.

Related pages: [1.0 Suitelets](../topics/ss1_api.suitelets.md),
[1.0 user events](../topics/ss1_api.user_events.md),
[1.0 client scripts](../topics/ss1_api.client_scripts.md),
[1.0 sublists](../topics/ss1_api.sublists.md),
[assistants, lists, portlets and template rendering in 1.0](objects-ui-other.md).

How to read this page:

- 1.0 methods take **positional** arguments. Each parameter table lists them in the order the call
  expects; swapping two arguments silently changes the meaning, so keep that order when repairing or
  converting a call.
- Sublist line numbers start at 1, not 0.
- Every method on this page has a documented cost of 0 usage units.
- The source gives no release ("since") for the objects themselves. Where it gives one for a method,
  that method shows a **Since:** line; a method without one has no release stated in the source.
- Many `nlobjField` setters only work on fields a script created through the UI objects
  (`nlobjForm.addField`, `nlobjSubList.addField` and similar), not on standard fields; each such
  method says so.
- Several methods work on "listed" built-in buttons. The 1.0 entries do not enumerate those buttons
  themselves. The generated [buttons catalog](../gen/catalogs/buttons.md) lists built-in button IDs
  with their UI labels for the 2.x `Form.removeButton`; it is the closest list available, but the
  source does not say it is the same list for 1.0.

## Objects

### nlobjForm (object)
<!-- id: ss1:nlobjForm -->

A page-level form. A Suitelet gets a fresh one from `nlapiCreateForm` and writes it to the
response; a `beforeLoad` user event receives the form of the record being shown as its second
argument (after `type`, before `request`). The form owns the fields, field groups, tabs, subtabs,
sublists and buttons on the page. The 2.x counterpart is the `Form` object of
[N/ui/serverWidget](../modules/N_ui_serverWidget.md).

A naming rule shared by several methods below (`addButton`, `addField`, `addSubList`, `addSubTab`,
`addTab`): the name you give must be lowercase without spaces, and when the element is added to an
existing NetSuite page (as opposed to a page your Suitelet builds itself) it must start with
`custpage`.

#### nlobjForm.addButton(name, label, script)
<!-- id: ss1:nlobjForm#addButton -->

Puts a button on the form and hands it back so you can adjust it further.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `label` | string | Yes | |
| `script` | string | No | |

- `name`: lowercase, no spaces; prefix with `custpage` when the form is an existing NetSuite page.
- `script`: client-side code that runs when the button is clicked (the onclick script).

**Returns:** `nlobjButton`, the new button.

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.addCredentialField(id, label, website, scriptId, value, entityMatch, tab)
<!-- id: ss1:nlobjForm#addCredentialField -->

Adds a field in which a user types a credential, such as a password for a third-party service. The
field does not keep the clear text; what it stores is an encrypted handle to the credential. The
platform guarantees three things: the script never sees the clear-text value, the value is never
written to logs, and it travels over SSL when exchanged with the third party. The handle is meant to
be consumed by `nlapiRequestURLWithCredentials`.

> [!SOURCE-ERROR] The source writes the related function's name with "url" in lowercase rather than
> the `URL` spelling that `nlapiRequestURLWithCredentials` has in its own entry; treat the lowercase
> form as a typo and use the function's own entry.
<!-- source-error: ss1:nlobjForm#addCredentialField | - | api_name_written_with_lowercase_url -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `id` | string | Yes | |
| `label` | string | Yes | |
| `website` | string | No | |
| `scriptId` | string | No | |
| `value` | string | No | |
| `entityMatch` | boolean | No | |
| `tab` | string | No | |

- `website`: one domain or an array of domains for which the credential may be used. Optional in
  general, but required when the credential will be passed to `nlapiRequestURLWithCredentials`.
- `scriptId`: the script ID (a `customscript_...` style identifier) of the script allowed to use
  the credential. Also required when the credential will be used with
  `nlapiRequestURLWithCredentials`.
- `value`: the field's initial value, which is a credential handle (not clear text).
- `entityMatch`: `true` means only the entity who entered the credential may use it.
- `tab`: a tab ID or a field group ID to place the field in; null puts it in the main section.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Availability:** client and server scripts; cannot be used in Suitelets deployed as available
without login.

**Since:** 2012.1

#### nlobjForm.addField(name, type, label, source, tab)
<!-- id: ss1:nlobjForm#addField -->

Adds a scripted field to the form. Together with `beforeLoad` user events on record forms, this is
how custom `nlobjField` objects come into being.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its own
> parameter list. This page follows the parameter list; rely on the positions in the table and check
> an existing call against them.
<!-- source-error: ss1:nlobjForm#addField | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | No | |
| `source` | number \| string | No | |
| `tab` | string | No | |

- `name`: lowercase, no spaces; prefix with `custpage` on an existing NetSuite page.
- `type`: one of `text`, `radio`, `label`, `email`, `phone`, `date`, `datetimetz`, `currency`,
  `float`, `integer`, `checkbox`, `select`, `url`, `timeofday`, `textarea`, `multiselect`, `image`,
  `inlinehtml`, `password`, `help`; `percent`, `longtext`, `richtext` and `file` are also accepted.
  `image` only works on sublists of type `list` or `staticlist`. `file` only works in Suitelets, and
  adding one switches the form to multipart encoding; the upload is read on POST with
  [`nlobjRequest.getFile`](objects-services.md).
- `source`: for `select` and `multiselect` fields, the ID of the list or record that supplies the
  options; for `radio` fields, the ID of this radio option. A select field with a source takes its
  options from it, so you cannot add options to it with `addSelectOption`.
- `tab`: a tab ID or a field group ID; null puts the field in the main section.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.addFieldGroup(name, label, tab)
<!-- id: ss1:nlobjForm#addFieldGroup -->

Adds a field group: a labelled block that fields join when its name is passed as their `tab`
argument.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `label` | string | Yes | |
| `tab` | string | No | |

- `tab`: the tab to put the group on; null puts it in the main area.

**Returns:** `nlobjFieldGroup`

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjForm.addPageLink(type, title, url)
<!-- id: ss1:nlobjForm#addPageLink -->

Adds a navigation link at the top of the form.

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

#### nlobjForm.addResetButton(label)
<!-- id: ss1:nlobjForm#addResetButton -->

Adds a button that resets the form's fields.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | No | |

- `label`: when omitted, the button reads "Reset".

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.addSubList(name, type, label, tab)
<!-- id: ss1:nlobjForm#addSubList -->

Adds a sublist (a grid of lines) to the form. Note that the UI switches off column sorting for
`staticlist` sublists holding more than 25 rows.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | Yes | |
| `tab` | string | No | |

- `name`: lowercase, no spaces; prefix with `custpage` on an existing NetSuite page.
- `type`: `editor` (lines edited one at a time, not inline), `inlineeditor` (edited inline), `list`
  (an editable list) or `staticlist` (read-only).
- `tab`: the tab to put the sublist on; null puts it on the main tab.

**Returns:** `nlobjSubList`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.addSubmitButton(label)
<!-- id: ss1:nlobjForm#addSubmitButton -->

Adds the button that submits the form.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | No | |

- `label`: when omitted, the button reads "Save".

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.addSubTab(name, label, tab)
<!-- id: ss1:nlobjForm#addSubTab -->

Adds a subtab. A tab that ends up with a single subtab does not show that subtab's label, so add at
least two when the labels matter.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `label` | string | Yes | |
| `tab` | string | No | |

- `name`: lowercase, no spaces; prefix with `custpage` on an existing NetSuite page.
- `tab`: the tab the subtab belongs to; null adds it to the main tab.

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.addTab(name, label)
<!-- id: ss1:nlobjForm#addTab -->

Adds a top-level tab.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `label` | string | Yes | |

- `name`: lowercase, no spaces; prefix with `custpage` on an existing NetSuite page.

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.getButton(name)
<!-- id: ss1:nlobjForm#getButton -->

Looks up a button on the form by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

- `name`: lowercase, no spaces.

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.getField(name, radio)
<!-- id: ss1:nlobjForm#getField -->

Looks up a field on the form by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `radio` | string | No | |

- `radio`: the ID of one radio option; it must be passed when the field is a radio field.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.insertField(field, nextfield)
<!-- id: ss1:nlobjForm#insertField -->

Moves a field so that it sits immediately before another, named field.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjForm#insertField | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `field` | nlobjField | Yes | |
| `nextfield` | string | Yes | |

- `nextfield`: the name of the field that `field` is placed in front of.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.insertSubList(sublist, nextsub)
<!-- id: ss1:nlobjForm#insertSubList -->

Places a sublist immediately before the named sublist or subtab.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `sublist` | nlobjSubList | Yes | |
| `nextsub` | string | Yes | |

**Returns:** `nlobjSubList`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.insertSubTab(name, nextsub)
<!-- id: ss1:nlobjForm#insertSubTab -->

Places a subtab immediately before the named sublist or subtab.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list, whose first argument is a string `name`;
> check existing calls by position.
<!-- source-error: ss1:nlobjForm#insertSubTab | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `nextsub` | string | Yes | |

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.insertTab(tab, nexttab)
<!-- id: ss1:nlobjForm#insertTab -->

Places a tab immediately before the named tab.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `tab` | nlobjTab | Yes | |
| `nexttab` | string | Yes | |

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.removeButton(name)
<!-- id: ss1:nlobjForm#removeButton -->

Takes a button off the form. It works on custom buttons and on the built-in buttons the source
lists. The 1.0 entry does not enumerate those built-in buttons; see the note at the top of this page
about the generated [buttons catalog](../gen/catalogs/buttons.md).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

- `name`: a custom button's name, or the ID of a built-in button.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.setFieldValues(values)
<!-- id: ss1:nlobjForm#setFieldValues -->

Sets several field values at once from an object that maps field names to values. Its usual home is
a `beforeLoad` user event, either to initialize a new record or to fill fields that are not stored.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `values` | Object | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.setScript(script)
<!-- id: ss1:nlobjForm#setScript -->

Attaches a global client script to the form; that script is where button onclick handlers normally
live.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `script` | string \| number | Yes | |

- `script`: the script ID or internal ID of a global client script.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.getSubList(name)
<!-- id: ss1:nlobjForm#getSubList -->

Looks up a sublist on the form by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjSubList`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.getSubTab(name)
<!-- id: ss1:nlobjForm#getSubTab -->

Looks up a subtab on the form by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.getTab(name)
<!-- id: ss1:nlobjForm#getTab -->

Looks up a tab on the form by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjForm.getTabs()
<!-- id: ss1:nlobjForm#getTabs -->

Returns every tab on the form.

**Returns:** `nlobjTab[]`

**Governance:** 0 units.

**Since:** 2012.2

#### nlobjForm.setTitle(title)
<!-- id: ss1:nlobjForm#setTitle -->

Sets the form's title.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `title` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

### nlobjField (object)
<!-- id: ss1:nlobjField -->

A field on a form or sublist. Scripted fields come into being in two ways: from a `beforeLoad` user
event on a record form, or through `nlobjForm.addField` in a Suitelet. A field obtained in a client
script through `nlapiGetField` is read-only: only its getters may be used there.

Reading the select options of a disabled field raises a permission error. The source does not give
the error code for that case.

#### nlobjField.addSelectOption(value, text, selected)
<!-- id: ss1:nlobjField#addSelectOption -->

Adds an option to a select field. Only select fields without a source accept options; a sourced
select draws its options from the source, so this method cannot add to it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `value` | string | Yes | |
| `text` | string | Yes | |
| `selected` | boolean | No | |

- `selected`: `true` makes this option the one selected by default.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.getSelectOptions(filter, filteroperator)
<!-- id: ss1:nlobjField#getSelectOptions -->

Reads the options of a select or radio field, optionally narrowed by a text filter. It returns at
most 1,000 options, is supported on server-side record objects, and what it returns can differ with
the role running the script. It returns null when the field is not a select field or does not exist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `filter` | string | No | |
| `filteroperator` | string | No | |

- `filter`: the text to filter the options by; matching ignores case.
- `filteroperator`: `contains`, `is` or `startswith`; when omitted, `contains` is used.

**Returns:** `nlobjSelectOption[]` (see [nlobjSelectOption](objects-record-search.md)), or null as
described above.

**Errors:**

- A permission error is raised when the field is disabled (the source gives no code).

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjField.setAlias(alias)
<!-- id: ss1:nlobjField#setAlias -->

Gives the field an alias; until you set one, the alias is the field's name. Only for fields scripted
through the UI objects. Returns the same field, so calls can be chained.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `alias` | string | Yes | |

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setBreakType(breaktype)
<!-- id: ss1:nlobjField#setBreakType -->

Controls whether the field starts a new column or row in the form layout. Only for fields scripted
through the UI objects. Returns the same field for chaining.

> [!SOURCE-ERROR] The source's description of this method appears to be copied from
> `setLayoutType`. Rely on the parameter values below, which concern column and row breaks.
<!-- source-error: ss1:nlobjField#setBreakType | - | description_copied_from_setlayouttype -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `breaktype` | string | Yes | |

- `breaktype`: `startcol`, `startrow` or `none` (`none` when not set). `startcol` begins a new
  column and turns off automatic column balancing; `startrow` begins a new row, meant for fields
  placed outside the normal column layout.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjField.setDefaultValue(value)
<!-- id: ss1:nlobjField#setDefaultValue -->

Sets the value the field starts with. Only for fields scripted through the UI objects. Returns the
same field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `value` | string | Yes | |

- `value`: an empty string leaves the field blank.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setDisplaySize(width, height)
<!-- id: ss1:nlobjField#setDisplaySize -->

Sets how large a field appears. Only for fields scripted through the UI objects; it applies to
multiselect, long text, rich text and text input fields and is not supported on fields of list
records. Returns the same field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `width` | number | Yes | |
| `height` | number | No | |

- `width`: measured in columns for a text area, in characters for other fields.
- `height`: measured in rows, for text areas and multiselect fields.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setDisplayType(type)
<!-- id: ss1:nlobjField#setDisplayType -->

Changes how the field is displayed. Not supported in client scripts. Returns the same field for
chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

- `type`: `inline`, `hidden`, `readonly`, `entry`, `disabled` or `normal` (`normal` when never set).
  `readonly` makes the field uneditable while its text can still be selected; `entry` is for an input
  field on a sublist. To show a hidden field again, set it back to `normal`.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Availability:** server scripts only (not client scripts).

**Since:** 2008.2

#### nlobjField.setHelpText(help, inline)
<!-- id: ss1:nlobjField#setHelpText -->

Sets the field-level help for the field. Returns the same field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `help` | string | Yes | |
| `inline` | boolean | No | `false` |

- `inline`: `true` also prints the help text directly under the field; this applies to assistant
  fields only.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjField.setLabel(label)
<!-- id: ss1:nlobjField#setLabel -->

Changes the field's label. Only for fields scripted through the UI objects. Returns the same field
for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | Yes | |

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setLayoutType(type, breaktype)
<!-- id: ss1:nlobjField#setLayoutType -->

Positions the field relative to the rest of the form layout. Only for fields scripted through the UI
objects. Returns the same field for chaining.

> [!SOURCE-ERROR] The source describes this method as setting the display type, which is the job of
> `setDisplayType`. Rely on the parameter values below, which are layout positions.
<!-- source-error: ss1:nlobjField#setLayoutType | - | description_says_display_type -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |
| `breaktype` | string | Yes | |

- `type`: `outside`, `outsidebelow`, `outsideabove`, `startrow`, `midrow`, `endrow` or `normal`
  (`normal` when not set).
- `breaktype`: `startcol`, `startrow` or `none` (`none` when not set).

> [!SOURCE-ERROR] The source marks `breaktype` as required but describes it as optional. Passing it
> explicitly (for example `'none'`) is safe under either reading.
<!-- source-error: ss1:nlobjField#setLayoutType | breaktype | marked_required_but_described_optional -->

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setLinkText(text)
<!-- id: ss1:nlobjField#setLinkText -->

Sets the text shown for the link of a URL field. Returns the same field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `text` | string | Yes | |

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setMandatory(mandatory)
<!-- id: ss1:nlobjField#setMandatory -->

Makes the field mandatory or optional. Only for fields scripted through the UI objects. Returns the
same field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `mandatory` | boolean | Yes | |

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setMaxLength(maxlength)
<!-- id: ss1:nlobjField#setMaxLength -->

Caps how many characters the field accepts. Only for fields scripted through the UI objects, of
type text, rich text, long text or text area. Returns the same field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `maxlength` | number | Yes | |

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setPadding(padding)
<!-- id: ss1:nlobjField#setPadding -->

Adds vertical space above the field. Only for fields scripted through the UI objects. Returns the
same field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `padding` | number | Yes | |

- `padding`: the number of empty rows above the field.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjField.setRichTextHeight(height)
<!-- id: ss1:nlobjField#setRichTextHeight -->

Sets the height of a rich text field's editor. Rich text editing must be enabled. Returns the same
field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `height` | number | No | |

- `height`: in pixels, from 100 to 500.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2015.1

#### nlobjField.setRichTextWidth(width)
<!-- id: ss1:nlobjField#setRichTextWidth -->

Sets the width of a rich text field's editor. Rich text editing must be enabled. Returns the same
field for chaining.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `width` | number | No | |

- `width`: in pixels, from 250 to 800.

**Returns:** `nlobjField` (the same field).

**Governance:** 0 units.

**Since:** 2015.1

#### nlobjField.getLabel()
<!-- id: ss1:nlobjField#getLabel -->

Returns the field's label.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjField.getName()
<!-- id: ss1:nlobjField#getName -->

Returns the field's internal name.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjField.getType()
<!-- id: ss1:nlobjField#getType -->

Returns the field's type.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2009.1

### nlobjFieldGroup (object)
<!-- id: ss1:nlobjFieldGroup -->

A labelled group of fields, used both on `nlobjForm` (from `nlobjForm.addFieldGroup`) and on
`nlobjAssistant` (see [objects-ui-other.md](objects-ui-other.md)).

#### nlobjFieldGroup.setCollapsible(collapsible, hidden)
<!-- id: ss1:nlobjFieldGroup#setCollapsible -->

Lets the user collapse the group. This works on assistant field groups only; field groups of an
`nlobjForm` do not support it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `collapsible` | boolean | Yes | |
| `hidden` | boolean | No | `false` |

- `collapsible`: `true` makes the group collapsible.
- `hidden`: `true` shows the group collapsed when the page loads; ignored when `collapsible` is
  `false`.

**Returns:** `nlobjFieldGroup`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjFieldGroup.setShowBorder(show)
<!-- id: ss1:nlobjFieldGroup#setShowBorder -->

Shows or hides the group's title and frame.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `show` | boolean | Yes | |

- `show`: `true` shows the title and the frame around the group.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjFieldGroup.setSingleColumn(column)
<!-- id: ss1:nlobjFieldGroup#setSingleColumn -->

Chooses between a single-column layout and automatic columns.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `column` | boolean | Yes | |

- `column`: `true` lays the group out in one column; `false` lets the platform spread its fields
  over one to three columns automatically.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2011.1

#### nlobjFieldGroup.setLabel(label)
<!-- id: ss1:nlobjFieldGroup#setLabel -->

Changes the group's label.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | Yes | |

**Returns:** `nlobjFieldGroup`

**Governance:** 0 units.

**Since:** 2009.2

### nlobjTab (object)
<!-- id: ss1:nlobjTab -->

A tab or subtab on a form, returned by `nlobjForm.addTab` and `nlobjForm.addSubTab`.

#### nlobjTab.setLabel(label)
<!-- id: ss1:nlobjTab#setLabel -->

Changes the tab's label.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | Yes | |

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjTab.setHelpText(help)
<!-- id: ss1:nlobjTab#setHelpText -->

Sets help text for the tab.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `help` | string | Yes | |

**Returns:** `nlobjTab`

**Governance:** 0 units.

**Since:** 2008.2

### nlobjButton (object)
<!-- id: ss1:nlobjButton -->

A button on a form. Custom buttons appear only in edit mode, and the usual place to add or adjust
them is a `beforeLoad` user event. Buttons in the "More Actions" menu cannot be added or removed this
way.

#### nlobjButton.setDisabled(disabled)
<!-- id: ss1:nlobjButton#setDisabled -->

Greys out a button. Only works on custom buttons.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `disabled` | boolean | No | |

- `disabled`: `true` greys the button out.

> [!SOURCE-ERROR] The source does not say whether `disabled` is required; the spec records it as
> optional only because nothing is stated. Pass the value explicitly.
<!-- source-error: ss1:nlobjButton#setDisabled | disabled | param_requiredness_not_stated -->

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjButton.setLabel(label)
<!-- id: ss1:nlobjButton#setLabel -->

Changes a button's label. Works on custom buttons and on the standard buttons the source lists (see
the note at the top of this page).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | No | |

> [!SOURCE-ERROR] The source does not say whether `label` is required; the spec records it as
> optional only because nothing is stated. Pass the new label explicitly.
<!-- source-error: ss1:nlobjButton#setLabel | label | param_requiredness_not_stated -->

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjButton.setVisible(visible)
<!-- id: ss1:nlobjButton#setVisible -->

Shows or hides a button. Works on custom buttons and on some standard buttons; the source does not
say which standard ones.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `visible` | boolean | No | `true` |

- `visible`: `false` hides the button.

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2010.2

### nlobjSubList (object)
<!-- id: ss1:nlobjSubList -->

A sublist on a form or an assistant. The source records it as returned by `nlobjForm.addSubList`;
`nlobjAssistant.addSubList` is also declared to return one (see
[objects-ui-other.md](objects-ui-other.md)), and assistant sublists are always of type
`inlineeditor`. A sublist that was not created through the UI objects is read-only.

> [!SOURCE-ERROR] The source misspells the object name in one place (with the "j" missing) and, on
> its API map page, spells it with different capitalization. The name is `nlobjSubList`.
<!-- source-error: ss1:nlobjSubList | - | object_name_written_nlobsublist -->
<!-- source-error: ss1:nlobjSubList | - | map_page_spells_object_name_in_other_case -->

#### nlobjSubList.addField(name, type, label, source)
<!-- id: ss1:nlobjSubList#addField -->

Adds a column field to the sublist. As on forms, a select field that has a source takes its options
from it, so you cannot add options to it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `label` | string | Yes | |
| `source` | number \| string | No | |

- `type`: one of `text`, `email`, `phone`, `date`, `datetimetz`, `currency`, `float`, `integer`,
  `checkbox`, `select`, `url`, `image`, `timeofday`, `textarea`, `percent`, `radio`. `image` works
  only on `list` or `staticlist` sublists; `radio` only on `list` sublists.
- `source`: for select fields, the list or record that supplies the options.

**Returns:** `nlobjField`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjSubList.addMarkAllButtons()
<!-- id: ss1:nlobjSubList#addMarkAllButtons -->

Adds the mark-all buttons to the sublist. Applies to `list` sublists and needs a checkbox column.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjSubList.addRefreshButton()
<!-- id: ss1:nlobjSubList#addRefreshButton -->

Adds a button that refreshes the sublist without reloading the page. Applies to `list` and
`staticlist` sublists.

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2009.1

#### nlobjSubList.getLineItemCount()
<!-- id: ss1:nlobjSubList#getLineItemCount -->

Returns the number of lines. Lines are numbered from 1, so the count is also the number of the last
line.

**Returns:** `number`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjSubList.getLineItemValue(group, fldnam, linenum)
<!-- id: ss1:nlobjSubList#getLineItemValue -->

Reads one value from a line. For custom column fields, the value is available only when the field is
set to show on the form; hidden fields are always available.

> [!SOURCE-ERROR] The source's text for this getter says it cannot be used to set default values,
> which reads as if copied from a setter. Treat it as a plain read.
<!-- source-error: ss1:nlobjSubList#getLineItemValue | - | getter_text_says_cannot_set_default_values -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `group` | string | Yes | |
| `fldnam` | string | Yes | |
| `linenum` | number | Yes | |

- `linenum`: 1-based line number.

**Returns:** `string`

**Governance:** 0 units.

**Since:** 2010.1

#### nlobjSubList.setAmountField(field)
<!-- id: ss1:nlobjSubList#setAmountField -->

Designates a column whose values are added up into a running total for the sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `field` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

The source states no release for this method.

#### nlobjSubList.setDisplayType(type)
<!-- id: ss1:nlobjSubList#setDisplayType -->

Hides or shows the sublist. Applies to scripted sublists and to `staticlist` sublists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `type` | string | Yes | |

- `type`: `hidden` or `normal` (`normal` when never set).

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjSubList.setLineItemValue(name, linenum, value)
<!-- id: ss1:nlobjSubList#setLineItemValue -->

Writes one value into a line. For custom column fields, the field must be set to show on the form;
hidden fields are always available.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `linenum` | number | Yes | |
| `value` | string | Yes | |

- `linenum`: 1-based line number.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjSubList.setLineItemValues(values)
<!-- id: ss1:nlobjSubList#setLineItemValues -->

Fills the sublist in one call from an array of search results or plain objects. When the rows are
search results, each line also receives the record type (`recordtype`), the internal ID (`id`) and,
for each field, a `<fieldname>_display` value carrying its display text.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `values` | nlobjSearchResult[] \| Object[] | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjSubList.setUniqueField(name)
<!-- id: ss1:nlobjSubList#setUniqueField -->

Marks a column whose values must be unique across lines; duplicates are rejected. Applies to
`inlineeditor` and `editor` sublists.

> [!SOURCE-ERROR] The source's API map note describes the 2.x target as a property, while the target
> it names is a method. Check the 2.x side in the conversion guide before relying on either form.
<!-- source-error: ss1:nlobjSubList#setUniqueField | - | map_note_says_uniquefieldid_property_target_is_method -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |

**Returns:** `nlobjField`

**Governance:** 0 units.

**Since:** 2009.2

#### nlobjSubList.addButton(name, type, script)
<!-- id: ss1:nlobjSubList#addButton -->

Adds a button to the sublist.

> [!SOURCE-ERROR] The source's heading for this method names the arguments differently from its
> parameter list. The table follows the parameter list; rely on argument position.
<!-- source-error: ss1:nlobjSubList#addButton | - | heading_and_parameter_list_names_differ -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `name` | string | Yes | |
| `type` | string | Yes | |
| `script` | string | No | |

- `name`: lowercase, no spaces.
- `type`: despite its name, this is the label shown on the button in the UI.
- `script`: the name of the function to run on click.

**Returns:** `nlobjButton`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjSubList.setHelpText(help)
<!-- id: ss1:nlobjSubList#setHelpText -->

Sets help text for the sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `help` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjSubList.setLabel(label)
<!-- id: ss1:nlobjSubList#setLabel -->

Changes the sublist's label.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

### nlobjColumn (object)
<!-- id: ss1:nlobjColumn -->

A column of a list or portlet. The source records it as returned by `nlobjList.addColumn` and
`nlobjList.addEditColumn`; `nlobjPortlet.addColumn` and `nlobjPortlet.addEditColumn` are also
declared to return an `nlobjColumn` (see [objects-ui-other.md](objects-ui-other.md)).

#### nlobjColumn.addParamToURL(param, value, dynamic)
<!-- id: ss1:nlobjColumn#addParamToURL -->

Appends a query parameter to the column's link URL. `setURL` must have been called first.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `param` | string | Yes | |
| `value` | string | Yes | |
| `dynamic` | boolean | No | |

- `dynamic`: `true` means `value` is the alias of a row field, so each row gets its own value.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjColumn.setURL(url, dynamic)
<!-- id: ss1:nlobjColumn#setURL -->

Turns the column's values into links to a URL.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `url` | string | Yes | |
| `dynamic` | boolean | No | |

- `dynamic`: `true` means `url` is the alias of a row field, so each row links to its own URL.

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

#### nlobjColumn.setLabel(label)
<!-- id: ss1:nlobjColumn#setLabel -->

Changes the column heading.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `label` | string | Yes | |

**Returns:** `void`

**Governance:** 0 units.

**Since:** 2008.2

## Examples

A legacy Suitelet that lets a dispatcher look up support cases. The GET branch draws a small filter
form; the POST branch also shows the matching cases in a read-only sublist filled straight from
search results. When converting, map this to `serverWidget.createForm` and an entry point that
receives a single context object.

```js
function caseLookupSuitelet(request, response) {
  var form = nlapiCreateForm('Case lookup');
  var group = form.addFieldGroup('custpage_filters', 'Filters');
  group.setSingleColumn(true);

  form.addField('custpage_from', 'date', 'Started on or after', null, 'custpage_filters')
    .setMandatory(true);

  var scope = form.addField('custpage_scope', 'select', 'Scope', null, 'custpage_filters');
  scope.addSelectOption('mine', 'My cases', true);
  scope.addSelectOption('all', 'All cases');

  if (request.getMethod() === 'POST') {
    var columns = [
      new nlobjSearchColumn('casenumber'),
      new nlobjSearchColumn('title'),
      new nlobjSearchColumn('startdate')
    ];
    var results = nlapiSearchRecord('supportcase', null, null, columns);

    var lines = form.addSubList('custpage_cases', 'staticlist', 'Matching cases');
    lines.addField('casenumber', 'text', 'Case');
    lines.addField('title', 'text', 'Subject');
    lines.addField('startdate', 'date', 'Started');
    if (results) {
      lines.setLineItemValues(results);
    }
    form.setFieldValues({ custpage_from: request.getParameter('custpage_from') });
  }

  form.addSubmitButton('Find cases');
  response.writePage(form);
}
```

A `beforeLoad` user event on purchase orders that adds an explanatory note and a custom button while
the record is edited, and removes a built-in button (`makecopy`, which the generated buttons catalog
labels "Make Copy"). Custom buttons only appear in edit mode, so the script checks `type` first; the
click handler lives in a global client script attached with `setScript`.

```js
function purchaseOrderBeforeLoad(type, form, request) {
  if (type !== 'edit') {
    return;
  }

  var note = form.addField('custpage_freight_note', 'inlinehtml', 'Freight note');
  note.setDefaultValue('<p>Freight is recalculated from the latest carrier rates.</p>');
  note.setLayoutType('outsideabove', 'none');

  form.setScript('customscript_po_freight_client');
  var recalc = form.addButton('custpage_recalc_freight', 'Recalculate freight', 'recalcFreight();');
  recalc.setDisabled(false);

  form.removeButton('makecopy');
}
```
