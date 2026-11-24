# N/ui/serverWidget
<!-- id: N/ui/serverWidget -->

`N/ui/serverWidget` builds pages that look and behave like native NetSuite screens: forms with
fields, field groups, tabs, subtabs, sublists, buttons and links; standalone lists; and multi-step
assistants (wizards). It is meant to be used with the [Suitelet](../script-types/Suitelet.md), which
creates a page object and writes it to the response. The `Form`, `Field`, `Sublist`, `Tab`,
`FieldGroup` and `Button` objects are also available in a
[user event](../script-types/UserEventScript.md) `beforeLoad`, which is how you customize an existing
record page before it is shown.

Things that hold across the whole module:

- **Server only.** Everything here runs in server scripts.
- **No DOM access.** You cannot reach into the NetSuite UI through the browser DOM; change pages
  through these SuiteScript APIs instead.
- **The `custpage` prefix.** When you add a UI object (field, button, tab, subtab, sublist...) to a
  page that already exists, such as a record form in `beforeLoad`, its ID must start with
  `custpage`. IDs are lowercase with no spaces.
- **User events: `beforeLoad` only.** Members marked available to user event scripts can be used
  only inside the `beforeLoad` entry point. Members marked Suitelet-only (`createForm`,
  `createList`, `createAssistant`, and the `Assistant`, `AssistantStep`, `List` and `ListColumn`
  objects) are not available in user events.
- **Governance.** Every callable in this module costs 0 units.
- **Errors.** The source documents error codes for only a few members (they are listed in those
  sections). For every other callable here, the source does not state which errors it can throw.

- **Load it as:** `define(['N/ui/serverWidget'], (serverWidget) => { ... })`
- Related: [N/http](N_http.md) (`ServerResponse.writePage` renders these objects),
  [N/ui/message](N_ui_message.md), [N/https](N_https.md), [N/sftp](N_sftp.md),
  [button catalog](../gen/catalogs/buttons.md).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`serverWidget.createForm(options)`](#serverwidgetcreateformoptions) | function | New NetSuite-style form (Suitelet) |
| [`serverWidget.createList(options)`](#serverwidgetcreatelistoptions) | function | New standalone list page (Suitelet) |
| [`serverWidget.createAssistant(options)`](#serverwidgetcreateassistantoptions) | function | New multi-step assistant (Suitelet) |
| [`Form`](#form-object) | object | A form: fields, links, buttons, tabs, sublists |
| [`Field`](#field-object) | object | A body or sublist field |
| [`Sublist`](#sublist-object) | object | A sublist on a form or assistant |
| [`FieldGroup`](#fieldgroup-object) | object | A labelled group of fields |
| [`Tab`](#tab-object) | object | A tab or subtab on a form |
| [`Button`](#button-object) | object | A button on a form or sublist |
| [`Assistant`](#assistant-object) | object | A multi-step wizard page |
| [`AssistantStep`](#assistantstep-object) | object | One step of an assistant |
| [`List`](#list-object) | object | A standalone list page |
| [`ListColumn`](#listcolumn-object) | object | A column of a list |
| [`serverWidget.AssistantSubmitAction`](#serverwidgetassistantsubmitaction) | enum | What the user did on an assistant page |
| [`serverWidget.FieldBreakType`](#serverwidgetfieldbreaktype) | enum | Column/row breaks for a field |
| [`serverWidget.FieldDisplayType`](#serverwidgetfielddisplaytype) | enum | How a field is shown |
| [`serverWidget.FieldLayoutType`](#serverwidgetfieldlayouttype) | enum | Where a field sits in the layout |
| [`serverWidget.FieldType`](#serverwidgetfieldtype) | enum | Field data types |
| [`serverWidget.FormPageLinkType`](#serverwidgetformpagelinktype) | enum | Where a page link appears |
| [`serverWidget.LayoutJustification`](#serverwidgetlayoutjustification) | enum | Column alignment in lists |
| [`serverWidget.ListStyle`](#serverwidgetliststyle) | enum | Visual style of a list |
| [`serverWidget.SublistDisplayType`](#serverwidgetsublistdisplaytype) | enum | Sublist visibility |
| [`serverWidget.SublistType`](#serverwidgetsublisttype) | enum | Sublist editing style |

## Functions

### serverWidget.createForm(options)
<!-- id: N/ui/serverWidget.createForm -->

Creates an empty form that you fill with fields, sublists, tabs and buttons and then render from a
Suitelet.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.hideNavBar` | Boolean | No | `false` |
| `options.title` | string | Yes | |

- `options.hideNavBar`: set to `true` to hide the navigation bar on the rendered page.

> [!SOURCE-ERROR] The source's description of `hideNavBar` here says it hides the *assistant*
> header, which does not fit a form. The spec treats it as hiding the navigation bar; check the
> rendered page if the exact effect matters.
<!-- source-error: N/ui/serverWidget.createForm | options.hideNavBar | says_assistant_header_hidden -->

**Returns:** `serverWidget.Form`, the new form.

**Governance:** 0 units.

**Availability:** server; Suitelet scripts.

**Since:** 2015.2

### serverWidget.createList(options)
<!-- id: N/ui/serverWidget.createList -->

Creates a standalone list page (rows and columns, not a form).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.hideNavBar` | Boolean | No | `false` |
| `options.title` | string | Yes | |

- `options.hideNavBar`: `true` hides the navigation bar. Listed since 2016.1.
- `options.title`: listed since 2016.1.

> [!SOURCE-ERROR] As with `createForm`, the source describes `hideNavBar` as hiding the assistant
> header, which does not match a list page. Rely on it hiding the navigation bar, and check the
> result in your account.
<!-- source-error: N/ui/serverWidget.createList | options.hideNavBar | says_assistant_header_hidden -->

> [!SOURCE-ERROR] The function is dated 2015.2, yet its required `title` parameter is dated 2016.1,
> which cannot both be true. The source does not say which date is right; do not rely on either
> date when targeting old releases.
<!-- source-error: N/ui/serverWidget.createList | - | required_param_since_2016.1_after_method_since_2015.2 -->

**Returns:** `serverWidget.List`, the new standalone list.

**Governance:** 0 units.

**Availability:** server; Suitelet scripts.

**Since:** 2015.2

### serverWidget.createAssistant(options)
<!-- id: N/ui/serverWidget.createAssistant -->

Creates a new assistant: a multi-step wizard page. Assistants are not supported in Suitelets that
are made available externally.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.title` | string | Yes | |
| `options.hideNavBar` | Boolean | No | `false` |

- `options.title`: shown on every page of the assistant.
- `options.hideNavBar`: `true` hides the navigation bar.

**Returns:** `serverWidget.Assistant`, the new assistant.

**Governance:** 0 units.

**Availability:** server; Suitelet scripts (not externally available Suitelets).

**Since:** 2015.2

## Objects

### Form (object)
<!-- id: N/ui/serverWidget.Form -->

A page built to look like a native NetSuite form. It can hold fields, links, buttons, tabs and
sublists. In a Suitelet you get one from [`serverWidget.createForm`](#serverwidgetcreateformoptions).
Available in Suitelet and User Event scripts (in user events, `beforeLoad` only), server side,
since 2015.2.

#### Form.addButton(options)
<!-- id: N/ui/serverWidget.Form#addButton -->

Adds a custom button to the form. Buttons can only be added from Suitelets and from user event
`beforeLoad`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.functionName` | string | No | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.functionName`: the name of a function in the custom-module client script attached to
  the form. A client script must be attached through
  [`Form.clientScriptModulePath`](#formclientscriptmodulepath) or
  [`Form.clientScriptFileId`](#formclientscriptfileid) for this to work. Listed since 2016.1.
- `options.id`: lowercase, no spaces; when adding to an existing page it must start with
  `custpage` (for instance a button labelled "Update Order" might use `custpage_updateorder`).

**Returns:** `serverWidget.Button`, the new [button](#button-object).

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.addCredentialField(options)
<!-- id: N/ui/serverWidget.Form#addCredentialField -->

Adds a field for storing credentials of a third-party system, such as a payment gateway login, so
scripts can use them with [N/sftp](N_sftp.md) or [N/https](N_https.md) without exposing them. The
platform guarantees that the value is never stored, displayed or logged in clear text, and that it
is exchanged in clear text only over TLS 1.2 or SFTP. The field yields a GUID; scripts named in
`restrictToScriptIds` turn it back into a usable secret with
[`https.createSecureString`](N_https.md). The GUID can be reused until the script runs again.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.restrictToDomains` | string \| string[] | Yes | |
| `options.restrictToScriptIds` | string \| string[] | Yes | |
| `options.restrictToCurrentUser` | Boolean | No | `false` |
| `options.container` | string | No | |
| `options.label` | string | Yes | |

- `options.id`: lowercase, no spaces; must start with `custpage` when adding to an existing page.
- `options.restrictToDomains`: the credential is sent only to these domains. Duplicate domains are
  not supported.
- `options.restrictToScriptIds`: scripts allowed to use the credential. Duplicate script IDs are
  not supported.
- `options.restrictToCurrentUser`: `true` means only the user who entered the credential may use
  it.
- `options.container`: ID of the tab or field group to place the field in; without it the field
  goes in the main section of the form. Listed since 2016.1.

**Returns:** `serverWidget.Field`, the new credential field.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

- The field's maximum length defaults to 32 characters; you can change it through
  [`Field.maxLength`](#fieldmaxlength).

#### Form.addField(options)
<!-- id: N/ui/serverWidget.Form#addField -->

Adds a body field to the form.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.source` | string | No | |
| `options.id` | string | Yes | |
| `options.container` | string | No | |
| `options.label` | string | Yes | |

- `options.type`: a [`serverWidget.FieldType`](#serverwidgetfieldtype) value. Long text fields
  added this way hold at most 100,000 characters (long text fields made with SuiteBuilder allow
  1,000,000).
- `options.source`: the internal ID or script ID of a list or record type that supplies the
  options of a select field, for example `salesorder`, `employeetype`, a custom record such as
  `customrecord_car_model`, or a custom list such as `customlist_color`. Once a source is set you
  cannot add your own options with [`Field.addSelectOption`](#fieldaddselectoptionoptions). For a
  radio field, `source` holds the internal ID of the field rather than a list.
- `options.id`: lowercase, no spaces, `custpage` prefix on existing pages (e.g.
  `custpage_purchase_details`).
- `options.container`: tab or field group ID; when omitted the field lands in the main section of
  the form. Listed since 2016.1.

**Returns:** `serverWidget.Field`, the new [field](#field-object).

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.addFieldGroup(options)
<!-- id: N/ui/serverWidget.Form#addFieldGroup -->

Adds a field group to the form.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tab` | string | No | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.tab`: the tab to put the group on; without it the group goes in the main section of the
  form.

**Returns:** `serverWidget.FieldGroup`, the new [group](#fieldgroup-object).

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.addPageInitMessage(options)
<!-- id: N/ui/serverWidget.Form#addPageInitMessage -->

Queues a banner message that appears once the page's `pageInit` has finished running. Call it from
a user event `beforeLoad` or a Suitelet. On record pages you control the record modes in which it
shows by checking the user event type ([`UserEventType`](../script-types/UserEventScript.md)) before
calling it. The message object and its type come from [N/ui/message](N_ui_message.md), so that
module is needed as well.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.title` | string | No | `""` |
| `options.message` | message.Message | Yes | |
| `options.type` | message.Type | Yes | |
| `options.duration` | int | No | `0` |

- `options.title`: listed since 2016.1.
- `options.message`: listed since 2018.2.
- `options.type`: listed since 2016.1.
- `options.duration`: how long the message stays, in milliseconds. `0` keeps it visible until it is
  hidden. A duration given when the message is shown overrides one set when it was created. Listed
  since 2018.2.

> [!SOURCE-ERROR] The source lists both a `message` parameter holding a `message.Message` object and
> one holding the message text as a string. The spec keeps the object form; check which form your
> account accepts before relying on either.
<!-- source-error: N/ui/serverWidget.Form#addPageInitMessage | - | lists_both_message_object_and_message_string_params -->

> [!SOURCE-ERROR] The `title` and `type` parameters are dated 2016.1, earlier than the method itself
> (2018.2). The source does not say which is right; assume the method and all its parameters exist
> only from 2018.2.
<!-- source-error: N/ui/serverWidget.Form#addPageInitMessage | - | param_since_2016.1_predates_method_since_2018.2 -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2018.2

#### Form.addPageLink(options)
<!-- id: N/ui/serverWidget.Form#addPageLink -->

Adds a link to the page. You pick the link type, but you cannot choose its exact position.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.title` | string | Yes | |
| `options.url` | string | Yes | |

- `options.type`: a [`serverWidget.FormPageLinkType`](#serverwidgetformpagelinktype) value.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.addResetButton(options)
<!-- id: N/ui/serverWidget.Form#addResetButton -->

Adds a button that clears every field on the form.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.label` | string | No | `Reset` |

**Returns:** `serverWidget.Button`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.addSecretKeyField(options)
<!-- id: N/ui/serverWidget.Form#addSecretKeyField -->

Adds a field that captures a secret key for the encryption or hashing done with the crypto modules.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.restrictToCurrentUser` | Boolean | No | `false` |
| `options.restrictToScriptIds` | string \| string[] | Yes | |
| `options.id` | string | Yes | |
| `options.container` | string | No | |
| `options.label` | string | Yes | |

- `options.restrictToCurrentUser`: `true` means only the user who entered the key may use it.
- `options.restrictToScriptIds`: the scripts allowed to use the key.
- `options.id`: lowercase, no spaces, `custpage` prefix on existing pages.
- `options.container`: tab or field group; defaults to the main section of the form.

**Returns:** `serverWidget.Field`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.1

- The maximum length defaults to 32 characters (see [`Field.maxLength`](#fieldmaxlength)).

#### Form.addSublist(options)
<!-- id: N/ui/serverWidget.Form#addSublist -->

Adds a sublist to the form. Sorting a static list sublist is not supported once it has more than
25 rows.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tab` | string | No | |
| `options.type` | string | Yes | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.tab`: the tab to place the sublist on; defaults to the main tab.
- `options.type`: a [`serverWidget.SublistType`](#serverwidgetsublisttype) value.
- `options.id`: lowercase, no spaces, `custpage` prefix on existing pages.

**Returns:** `serverWidget.Sublist`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.addSubmitButton(options)
<!-- id: N/ui/serverWidget.Form#addSubmitButton -->

Adds the button that submits the form.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.label` | string | No | `Save` |

> [!SOURCE-ERROR] The source page for this method carries a note about sublist sorting that appears
> to have been copied from `addSublist`; it has nothing to do with submit buttons. Ignore it here.
<!-- source-error: N/ui/serverWidget.Form#addSubmitButton | - | copied_sublist_sorting_note -->

**Returns:** `serverWidget.Button`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.1

#### Form.addSubtab(options)
<!-- id: N/ui/serverWidget.Form#addSubtab -->

Adds a subtab. A subtab stays hidden until at least one object (field, sublist...) is assigned to
it, and if it holds fewer than two fields those fields are shown at the bottom of the form instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tab` | string | No | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.tab`: the parent tab; defaults to the main tab.
- `options.id`: lowercase, no spaces, `custpage` prefix on existing pages (e.g.
  `custpage_purchasedetails`).

**Returns:** `serverWidget.Tab`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.addTab(options)
<!-- id: N/ui/serverWidget.Form#addTab -->

Adds a top-level tab. Like a subtab, a tab is not shown until something is assigned to it, and a tab
holding fewer than two fields has them shown at the bottom of the form. If the form ends up with a
single tab, its contents are shown without a tab bar.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.id`: lowercase, no spaces, `custpage` prefix on existing pages.

**Returns:** `serverWidget.Tab`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.getButton(options)
<!-- id: N/ui/serverWidget.Form#getButton -->

Looks up a button on the form by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: lowercase, no spaces.

**Returns:** `serverWidget.Button`, the button with that ID. The source does not state what is
returned when no button has that ID.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.getField(options)
<!-- id: N/ui/serverWidget.Form#getField -->

Looks up a field on the form by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: lowercase, no spaces.

**Returns:** `serverWidget.Field`, the field with that ID. The source does not state what is
returned when no field has that ID.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.getSublist(options)
<!-- id: N/ui/serverWidget.Form#getSublist -->

Looks up a sublist on the form by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: lowercase, no spaces.

**Returns:** `serverWidget.Sublist`, the sublist with that ID. The source does not state what is
returned when no sublist has that ID.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.getSubtab(options)
<!-- id: N/ui/serverWidget.Form#getSubtab -->

Looks up a subtab by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: lowercase, no spaces.

**Returns:** `serverWidget.Tab`, the subtab with that ID. The source does not state what is
returned when no subtab has that ID.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.getTab(options)
<!-- id: N/ui/serverWidget.Form#getTab -->

Looks up a tab by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.Tab`, the tab with that ID. The source does not state what is returned
when no tab has that ID.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.1

#### Form.getTabs()
<!-- id: N/ui/serverWidget.Form#getTabs -->

Lists the tabs on the form. Takes no parameters.

**Returns:** `string[]`, the IDs of all tabs.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.insertField(options)
<!-- id: N/ui/serverWidget.Form#insertField -->

Places a field in front of another field. It works on custom fields created in `beforeLoad`;
moving a field to a different tab is not supported.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.isBefore` | boolean | No | |
| `options.field` | serverWidget.Field | Yes | |
| `options.nextfield` | string | Yes | |

- `options.isBefore`: `true` inserts the field before `nextfield`. Since 2024.2. The source does
  not state its default.
- `options.field`: the field object to place. Listed since 2016.1.
- `options.nextfield`: ID of the field to insert next to. Listed since 2015.2.

> [!SOURCE-ERROR] The method is dated 2015.2, but its required `field` parameter is dated 2016.1,
> so the method could not have worked as documented before 2016.1. The source does not say which
> date is right; do not rely on this method in releases before 2016.1.
<!-- source-error: N/ui/serverWidget.Form#insertField | - | required_param_since_2016.1_after_method_since_2015.2 -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.insertSublist(options)
<!-- id: N/ui/serverWidget.Form#insertSublist -->

Places a sublist in front of another sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.sublist` | serverWidget.Sublist | Yes | |
| `options.nextsublist` | string | Yes | |

- `options.sublist`: the sublist object to place. Listed since 2016.1.
- `options.nextsublist`: ID of the sublist that should follow. Listed since 2015.2.

> [!SOURCE-ERROR] The required `sublist` parameter is dated 2016.1, later than the method itself
> (2015.2). The source does not explain the gap or say which date is right; treat the method as
> usable only from 2016.1 if old releases matter.
<!-- source-error: N/ui/serverWidget.Form#insertSublist | - | required_param_since_2016.1_after_method_since_2015.2 -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.insertSubtab(options)
<!-- id: N/ui/serverWidget.Form#insertSubtab -->

Places a subtab in front of another subtab.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.subtab` | serverWidget.Tab | Yes | |
| `options.nextsub` | string | Yes | |

- `options.subtab`: the subtab object to place. Listed since 2016.1.
- `options.nextsub`: ID of the subtab that should follow. Listed since 2015.2.

> [!SOURCE-ERROR] The required `subtab` parameter is dated 2016.1, after the method's own date
> (2015.2). The source leaves the conflict unresolved; do not count on the method before 2016.1.
<!-- source-error: N/ui/serverWidget.Form#insertSubtab | - | required_param_since_2016.1_after_method_since_2015.2 -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.insertTab(options)
<!-- id: N/ui/serverWidget.Form#insertTab -->

Places a tab in front of another tab.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tab` | serverWidget.Tab | Yes | |
| `options.nexttab` | string | Yes | |

- `options.tab`: the tab object to place. Listed since 2016.1.
- `options.nexttab`: ID of the tab that should follow. Listed since 2015.2.

> [!SOURCE-ERROR] Same date conflict as the other insert methods: the required `tab` parameter is
> dated 2016.1 while the method is dated 2015.2, and the source does not say which is right.
<!-- source-error: N/ui/serverWidget.Form#insertTab | - | required_param_since_2016.1_after_method_since_2015.2 -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.removeButton(options)
<!-- id: N/ui/serverWidget.Form#removeButton -->

Removes a button from the form. Works for custom buttons and for some standard ones; the
[button catalog](../gen/catalogs/buttons.md) lists which standard buttons are affected.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

- `options.id`: lowercase, no spaces.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Form.updateDefaultValues(values)
<!-- id: N/ui/serverWidget.Form#updateDefaultValues -->

Sets the default values of several fields in one call. The argument is passed directly, not inside
an `options` object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `values` | Object[] | Yes | |

- `values`: pairs of field name and value. Listed since 2016.1.

> [!SOURCE-ERROR] The source types `values` as an array of objects, while its syntax sample passes a
> single plain object mapping field IDs to values, and that sample has no key named `values` at all.
> The spec does not settle the shape; test whether your account expects one map or an array before
> relying on either.
<!-- source-error: N/ui/serverWidget.Form#updateDefaultValues | values | typed_Object_array_but_syntax_passes_plain_field_map -->
<!-- source-error: N/ui/serverWidget.Form#updateDefaultValues | values | param_named_values_but_syntax_has_no_values_key -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.1

#### Form.clientScriptFileId
<!-- id: N/ui/serverWidget.Form#clientScriptFileId -->

`number`. File Cabinet ID of a client script to attach to the page on demand, so a server script
can give its page client-side behaviour. Log entries written by that client script are recorded
against the parent (server) script's deployment. You can set this or
[`clientScriptModulePath`](#formclientscriptmodulepath), not both: setting one while the other is
already set throws `PROPERTY_VALUE_CONFLICT`. Suitelet and User Event (`beforeLoad` only), since
2015.2.

#### Form.clientScriptModulePath
<!-- id: N/ui/serverWidget.Form#clientScriptModulePath -->

`string`. Path of a client script module to attach on demand, the alternative to
[`clientScriptFileId`](#formclientscriptfileid). Client-side logs go to the parent script's
deployment. The two properties are mutually exclusive; setting this one while the file ID is
already set throws `PROPERTY_VALUE_CONFLICT`. Suitelet and User Event (`beforeLoad` only), since
2016.2.

#### Form.title
<!-- id: N/ui/serverWidget.Form#title -->

`string`. The form's title. Suitelet and User Event (`beforeLoad` only), since 2015.2.

### Field (object)
<!-- id: N/ui/serverWidget.Field -->

A field on a form body or in a sublist. Created by [`Form.addField`](#formaddfieldoptions),
[`Sublist.addField`](#sublistaddfieldoptions) and [`Assistant.addField`](#assistantaddfieldoptions).
Several properties below only affect fields created with this module, not standard fields already
on a record. Suitelet and User Event (`beforeLoad` only), server side, since 2015.2.

#### Field.addSelectOption(options)
<!-- id: N/ui/serverWidget.Field#addSelectOption -->

Adds one entry to a dropdown (select) field. You cannot use it on a field whose `source` was set
when the field was created; such a field takes its options from the source list.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | string | Yes | |
| `options.isSelected` | Boolean | No | `false` |
| `options.text` | string | Yes | |

- `options.value`: the internal ID of the option.
- `options.isSelected`: `true` makes this option the one selected by default.
- `options.text`: the text the user sees.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Field.getSelectOptions(options)
<!-- id: N/ui/serverWidget.Field#getSelectOptions -->

Reads the options of a select field as an array of ID and label pairs (for example an option with
value 5 and text "abc", another with value 6 and text "123").

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.filter` | string | No | |
| `options.filteroperator` | string | No | `contains` |

- `options.filter`: keeps only options whose text contains the filter text; matching ignores case.
  With the default operator, a filter of "john" would match both "John Smith" and "Shauna
  Johnson".
- `options.filteroperator`: one of `contains`, `is`, `startswith`.

**Returns:** `Object[]`, at most 500 options.

**Errors:**

- `TypeError`: the field's type does not support select options.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

- The method gives back `null` instead when the field is missing or is shown as a popup rather than
  a dropdown, even though the declared return type is an array. A dropdown turns into a popup once
  its option count exceeds the account's set preference for the maximum number of entries in
  dropdowns (the source does not give the preference's exact UI label).
- The options returned can differ depending on the user's role.

#### Field.setHelpText(options)
<!-- id: N/ui/serverWidget.Field#setHelpText -->

Sets the help text shown in a popup when the user clicks the field label.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.showInlineForAssistant` | Boolean | No | `false` |
| `options.help` | string | Yes | |

- `options.showInlineForAssistant`: for fields on assistants, `true` also prints the help text
  under the field. The spec marks its default of `false` as conditional without saying on what.

> [!SOURCE-ERROR] A note in the source refers to this parameter as `inline` instead of
> `showInlineForAssistant`. Use the name in the parameter table; the spec does not confirm any
> `inline` parameter.
<!-- source-error: N/ui/serverWidget.Field#setHelpText | options.showInlineForAssistant | note_calls_parameter_inline -->

**Returns:** `serverWidget.Field`, so calls can be chained.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Field.updateBreakType(options)
<!-- id: N/ui/serverWidget.Field#updateBreakType -->

Inserts a break in the form's flow layout at this field (for instance starting a new column).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.breakType` | string | Yes | |

- `options.breakType`: a [`serverWidget.FieldBreakType`](#serverwidgetfieldbreaktype) value.

**Returns:** `serverWidget.Field`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.1

#### Field.updateDisplaySize(options)
<!-- id: N/ui/serverWidget.Field#updateDisplaySize -->

Changes how big the field is drawn. Applies to multi-select, long text and text input fields. It
does not work on list/record fields or rich text fields; for rich text use
[`richTextWidth`](#fieldrichtextwidth) and [`richTextHeight`](#fieldrichtextheight) instead.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.height` | number | Yes | |
| `options.width` | number | Yes | |

- `options.height`: number of rows for text areas and multi-selects.
- `options.width`: columns for text areas; characters for other fields.

**Returns:** `serverWidget.Field`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Field.updateDisplayType(options)
<!-- id: N/ui/serverWidget.Field#updateDisplayType -->

Changes how the field is displayed (editable, inline text, hidden, disabled...).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.displayType` | string | Yes | |

- `options.displayType`: a [`serverWidget.FieldDisplayType`](#serverwidgetfielddisplaytype) value.

**Returns:** `serverWidget.Field`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.1

#### Field.updateLayoutType(options)
<!-- id: N/ui/serverWidget.Field#updateLayoutType -->

Changes where the field sits relative to the normal layout and neighbouring fields.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.layoutType` | string | Yes | |

- `options.layoutType`: a [`serverWidget.FieldLayoutType`](#serverwidgetfieldlayouttype) value.

**Returns:** `serverWidget.Field`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.1

#### Field.alias
<!-- id: N/ui/serverWidget.Field#alias -->

`string`. An alternate name for the field; unless you set it, it equals the field's internal ID.
Only for fields created with this module. Since 2015.2.

#### Field.defaultValue
<!-- id: N/ui/serverWidget.Field#defaultValue -->

`string`. The value the field starts with. Applies to fields created with this module. An empty
value, or a value that is not a number, leaves the field blank.

> [!SOURCE-ERROR] The source states the "non-number leaves the field blank" rule as if it applied
> to every field type, which cannot be right for text fields. Treat it as a numeric-field rule and
> test the behaviour for the field type you use.
<!-- source-error: N/ui/serverWidget.Field#defaultValue | - | non_number_rule_stated_for_all_field_types -->

Since 2015.2.

#### Field.helpText
<!-- id: N/ui/serverWidget.Field#helpText -->

`string`, read-only. The field's help text. To set it, use
[`setHelpText`](#fieldsethelptextoptions). The source does not state since which version this
property exists.

#### Field.id
<!-- id: N/ui/serverWidget.Field#id -->

`string`, read-only. The field's internal ID. Since 2015.2.

#### Field.isMandatory
<!-- id: N/ui/serverWidget.Field#isMandatory -->

`Boolean`. `true` makes the field required. Defaults to `false`. Only for fields created with this
module. Since 2015.2.

#### Field.label
<!-- id: N/ui/serverWidget.Field#label -->

`string`. The field label. Labels of custom fields can be at most 40 characters. Since 2015.2.

#### Field.linkText
<!-- id: N/ui/serverWidget.Field#linkText -->

`string`. Text displayed in place of the URL. Since 2015.2.

#### Field.maxLength
<!-- id: N/ui/serverWidget.Field#maxLength -->

`number`. Maximum number of characters the user can enter. Applies to text, rich text, long text
and text area fields created with this module. Since 2015.2.

#### Field.padding
<!-- id: N/ui/serverWidget.Field#padding -->

`number`. How many empty lines to leave above the field. Only for fields created with this module.
Since 2015.2.

#### Field.richTextHeight
<!-- id: N/ui/serverWidget.Field#richTextHeight -->

`number`. Height of a rich text field in pixels, between 100 and 500. Requires the
`Rich Text Editing` feature to be enabled. Since 2015.2.

#### Field.richTextWidth
<!-- id: N/ui/serverWidget.Field#richTextWidth -->

`number`. Width of a rich text field in pixels, between 250 and 800. Requires the
`Rich Text Editing` feature to be enabled. Since 2015.2.

> [!SOURCE-ERROR] The source's syntax sample sets this width to 100, which lies outside its own
> stated 250 to 800 pixel range. Stay within 250 to 800.
<!-- source-error: N/ui/serverWidget.Field#richTextWidth | - | syntax_sample_width_100_outside_250_to_800_range -->

#### Field.type
<!-- id: N/ui/serverWidget.Field#type -->

`string`, read-only. The field's type. The spec says its values come from
[`format.Type`](N_format.md); that looks inconsistent, since fields are created with
[`serverWidget.FieldType`](#serverwidgetfieldtype) values, and the spec does not explain it. It also
notes that select fields are limited to 801 characters. Since 2015.2.

### Sublist (object)
<!-- id: N/ui/serverWidget.Sublist -->

A sublist on a [`Form`](#form-object) or an [`Assistant`](#assistant-object). A sublist is
read-only unless it was created with this module in a Suitelet or in user event `beforeLoad`.
Suitelet and User Event (`beforeLoad` only), server side, since 2015.2.

#### Sublist.addButton(options)
<!-- id: N/ui/serverWidget.Sublist#addButton -->

Adds a button to the sublist. Only possible in Suitelets and user event `beforeLoad`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.functionName` | string | No | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.functionName`: name of a function in the custom-module client script attached via the
  form's `clientScriptModulePath` or `clientScriptFileId`; a client script must be attached.
- `options.id`: lowercase, no spaces.

**Returns:** `serverWidget.Button`, the new button.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.addField(options)
<!-- id: N/ui/serverWidget.Sublist#addField -->

Adds a column (field) to the sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.source` | string | No | |
| `options.id` | string | Yes | |
| `options.container` | string | No | |
| `options.label` | string | Yes | |

- `options.type`: a [`serverWidget.FieldType`](#serverwidgetfieldtype) value, except `DATETIME`,
  `FILE`, `HELP`, `INLINEHTML`, `LABEL`, `LONGTEXT`, `MULTISELECT`, `RADIO` and `RICHTEXT`, which
  sublists do not support.
- `options.source`: list or record that feeds a select column. Leave it null if you want to supply
  your own options; after a source is set you cannot call
  [`Field.addSelectOption`](#fieldaddselectoptionoptions).
- `options.id`: lowercase, no spaces.
- `options.container`: the spec says that, when omitted, the field goes to the main section of the
  form. It does not explain what a container means for a sublist column.

> [!SOURCE-ERROR] For this sublist method the source describes `container` in the same terms it uses
> for `Form.addField`, as a tab or field group on the form, which does not fit a column that lives
> inside a sublist. The source does not say what the parameter really does here; leave it out, or
> test its effect in your account before depending on it.
<!-- source-error: N/ui/serverWidget.Sublist#addField | options.container | sublist_field_container_described_as_form_tab_or_group -->

All parameters are listed since 2015.2.

**Returns:** `serverWidget.Field`, the new sublist field.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.addMarkAllButtons()
<!-- id: N/ui/serverWidget.Sublist#addMarkAllButtons -->

Adds "Mark All" and "Unmark All" buttons. Applies to sublists of type `LIST`. No parameters.

**Returns:** `serverWidget.Button[]`, the buttons added.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.addRefreshButton()
<!-- id: N/ui/serverWidget.Sublist#addRefreshButton -->

Adds a Refresh button. Applies to sublists of type `LIST`. No parameters.

**Returns:** `serverWidget.Button`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.getField(options)
<!-- id: N/ui/serverWidget.Sublist#getField -->

Looks up a sublist field by ID (for instance `item`).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.Field`. The source does not state what is returned when no field has
that ID.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2016.2

#### Sublist.getSublistValue(options)
<!-- id: N/ui/serverWidget.Sublist#getSublistValue -->

Reads the value of one field on one line of the sublist. Calling it on a subrecord field raises an
error.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.line` | number | Yes | |
| `options.id` | string | Yes | |

- `options.line`: line number, counting from 0.
- `options.id`: the sublist field ID.

**Returns:** `string`, the field value on that line.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: a required argument was not supplied.
- `YOU_CANNOT_CALL_1_METHOD_ON_SUBRECORD_FIELD_SUBLIST_2_FIELD_3`: the call failed, which happens
  when the field is a subrecord field.

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.insertField(options)
<!-- id: N/ui/serverWidget.Sublist#insertField -->

Places a sublist field in front of another field. Works for custom fields created in `beforeLoad`;
it cannot move fields between tabs.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.isBefore` | boolean | No | |
| `options.field` | serverWidget.Field | Yes | |
| `options.nextfield` | string | Yes | |

- `options.isBefore`: chooses whether the field goes before or after `nextfield`. The source does
  not state its default.
- `options.field`: the field to place.
- `options.nextfield`: ID of the field to insert next to.
- All three parameters are dated 2024.2, like the method.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2024.2

#### Sublist.setSublistValue(options)
<!-- id: N/ui/serverWidget.Sublist#setSublistValue -->

Writes a value into one field on one line of the sublist.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.line` | number | Yes | |
| `options.value` | string | Yes | |
| `options.id` | string | Yes | |

- `options.line`: line number, counting from 0.
- `options.value`: for checkbox fields pass the strings `'T'` or `'F'`, not booleans.
- `options.id`: the sublist field ID.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.updateTotallingFieldId(options)
<!-- id: N/ui/serverWidget.Sublist#updateTotallingFieldId -->

Turns a field into the sublist's running-total column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.Sublist`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.updateUniqueFieldId(options)
<!-- id: N/ui/serverWidget.Sublist#updateUniqueFieldId -->

Declares a field whose value must be unique across all rows, such as a date column where each date
may appear only once. Applies to `INLINEEDITOR` and `EDITOR` sublists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.Sublist`

**Governance:** 0 units.

**Availability:** server; Suitelet, User Event (`beforeLoad` only).

**Since:** 2015.2

#### Sublist.displayType
<!-- id: N/ui/serverWidget.Sublist#displayType -->

`string`. Whether and how the sublist is shown; a
[`serverWidget.SublistDisplayType`](#serverwidgetsublistdisplaytype) value. Since 2015.2.

#### Sublist.helpText
<!-- id: N/ui/serverWidget.Sublist#helpText -->

`string`. Inline help text shown for the sublist. Since 2015.2.

#### Sublist.label
<!-- id: N/ui/serverWidget.Sublist#label -->

`string`. The sublist's label. Since 2015.2.

#### Sublist.lineCount
<!-- id: N/ui/serverWidget.Sublist#lineCount -->

`number`, read-only. How many lines the sublist has. The spec also tags this property as 0-based,
which describes line numbering rather than a count and looks inconsistent; treat it as the number
of lines. Since 2015.2.

### FieldGroup (object)
<!-- id: N/ui/serverWidget.FieldGroup -->

A labelled box of fields used to title, hide or collapse a set of fields; its contents are laid out
in one or two columns. Available on [`Form`](#form-object) and [`Assistant`](#assistant-object).
Suitelet and User Event (`beforeLoad` only), server side, since 2015.2.

#### FieldGroup.isBorderHidden
<!-- id: N/ui/serverWidget.FieldGroup#isBorderHidden -->

`Boolean`. `true` hides the border around the group. Default `false`. Since 2015.2.

#### FieldGroup.isCollapsed
<!-- id: N/ui/serverWidget.FieldGroup#isCollapsed -->

`Boolean`. `true` shows the group collapsed when the page loads. Default `false`. Only works on
assistants.

> [!SOURCE-ERROR] The source says this property is for assistants only, yet lists user event
> `beforeLoad` among the places it can be used. Rely on it in assistants, and verify before
> depending on it on record forms.
<!-- source-error: N/ui/serverWidget.FieldGroup#isCollapsed | - | assistant_only_yet_listed_for_user_event_beforeload -->

Since 2015.2.

#### FieldGroup.isCollapsible
<!-- id: N/ui/serverWidget.FieldGroup#isCollapsible -->

`Boolean`. `true` lets the user collapse the group. Default `false`. Only works on assistants.

> [!SOURCE-ERROR] Same mismatch as `isCollapsed`: the property is described as assistant-only, but
> its availability also lists user event `beforeLoad`. Rely on it in assistants and test it before
> using it on record forms.
<!-- source-error: N/ui/serverWidget.FieldGroup#isCollapsible | - | assistant_only_yet_listed_for_user_event_beforeload -->

Since 2015.2.

#### FieldGroup.isSingleColumn
<!-- id: N/ui/serverWidget.FieldGroup#isSingleColumn -->

`Boolean`. `true` lays the group out in one column instead of two. Default `false`. Since 2015.2.

#### FieldGroup.label
<!-- id: N/ui/serverWidget.FieldGroup#label -->

`string`. The group's label. Since 2015.2.

### Tab (object)
<!-- id: N/ui/serverWidget.Tab -->

A tab or subtab on a form, created by [`Form.addTab`](#formaddtaboptions),
[`Form.addSubtab`](#formaddsubtaboptions), [`Form.insertTab`](#forminserttaboptions) or
[`Form.insertSubtab`](#forminsertsubtaboptions). Tab IDs are lowercase with no spaces and need the
`custpage` prefix on existing pages. A tab is hidden until at least one object is assigned to it,
and when it holds fewer than two fields they are shown at the bottom of the form. Suitelet and User
Event (`beforeLoad` only), server side, since 2015.2.

#### Tab.helpText
<!-- id: N/ui/serverWidget.Tab#helpText -->

`string`. Inline help text for the tab. Since 2015.2.

#### Tab.label
<!-- id: N/ui/serverWidget.Tab#label -->

`string`. The tab's label. Since 2015.2.

### Button (object)
<!-- id: N/ui/serverWidget.Button -->

A button created by [`Form.addButton`](#formaddbuttonoptions) or
[`Sublist.addButton`](#sublistaddbuttonoptions). On record pages, custom buttons appear to the left
of the printer icon; the recommended place to add them is user event
[`beforeLoad`](../script-types/UserEventScript.md). You cannot add or remove buttons in the Actions
menu with this module; use point-and-click form customization for those. Suitelet and User Event
(`beforeLoad` only), server side, since 2015.2.

#### Button.isDisabled
<!-- id: N/ui/serverWidget.Button#isDisabled -->

`Boolean`. `true` dims the button so it cannot be clicked. Default `false`. Custom buttons only.
Since 2015.2.

#### Button.isHidden
<!-- id: N/ui/serverWidget.Button#isHidden -->

`boolean`. `true` hides the button. Default `false`. Works for custom buttons and some standard
buttons (see the [button catalog](../gen/catalogs/buttons.md)). Since 2015.2.

#### Button.label
<!-- id: N/ui/serverWidget.Button#label -->

`string`. The button text; handy for relabelling a button depending on context. Works for custom
buttons and most standard buttons (see the [button catalog](../gen/catalogs/buttons.md)). Since
2015.2.

### Assistant (object)
<!-- id: N/ui/serverWidget.Assistant -->

A multi-step wizard, created by
[`serverWidget.createAssistant`](#serverwidgetcreateassistantoptions). Steps can run strictly in
order, or in any order with some steps optional. NetSuite tracks the assistant's state for the
user's session automatically. An assistant can hold fields, steps, buttons, tabs and sublists. Not
supported in externally available Suitelets. Suitelet only, server side, since 2015.2.

#### Assistant.addField(options)
<!-- id: N/ui/serverWidget.Assistant#addField -->

Adds a field to the assistant.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.source` | string | No | |
| `options.container` | string | No | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.type`: a [`serverWidget.FieldType`](#serverwidgetfieldtype) value. Long text holds at
  most 100,000 characters (SuiteBuilder long text allows 1,000,000). For a select or multi-select
  with your own options, leave `source` null.
- `options.source`: internal ID or script ID of the source list. Required for a radio field, where
  it holds the radio button's ID. After a source is set you cannot call
  [`Field.addSelectOption`](#fieldaddselectoptionoptions).
- `options.container`: ID of the field group to put the field in. Listed since 2016.1.
- `options.id`: radio buttons that belong to the same group share one `id`.

**Returns:** `serverWidget.Field`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.addFieldGroup(options)
<!-- id: N/ui/serverWidget.Assistant#addFieldGroup -->

Adds a field group for titling, hiding or collapsing fields, laid out in one or two columns. On
assistants a new group starts out collapsible and expanded; change that with
[`FieldGroup.isCollapsed`](#fieldgroupiscollapsed) and
[`FieldGroup.isCollapsible`](#fieldgroupiscollapsible).

> [!SOURCE-ERROR] "Collapsible by default" here contradicts `FieldGroup.isCollapsible`, whose
> documented default is `false`. The source does not say which is right; set `isCollapsible`
> explicitly if the behaviour matters.
<!-- source-error: N/ui/serverWidget.Assistant#addFieldGroup | - | default_collapsible_vs_iscollapsible_default_false -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

**Returns:** `serverWidget.FieldGroup`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.addStep(options)
<!-- id: N/ui/serverWidget.Assistant#addStep -->

Adds a step (a page) to the assistant. Whether steps must be done in order is controlled by
[`isNotOrdered`](#assistantisnotordered); step help text goes in
[`AssistantStep.helpText`](#assistantstephelptext).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.label` | string | Yes | |
| `options.id` | string | Yes | |

- `options.label`: by default the step labels are listed vertically in the left panel.
- `options.id`: for example `entercontacts`.

**Returns:** `serverWidget.AssistantStep`, the new step.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.addSublist(options)
<!-- id: N/ui/serverWidget.Assistant#addSublist -->

Adds a sublist. Assistants only accept inline editor sublists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.type`: must be `INLINEEDITOR` from [`serverWidget.SublistType`](#serverwidgetsublisttype).
  Listed since 2016.1.

> [!SOURCE-ERROR] The required `type` parameter is dated 2016.1, later than the method (2015.2).
> The source does not say which date is right; do not rely on this method in releases before
> 2016.1.
<!-- source-error: N/ui/serverWidget.Assistant#addSublist | - | required_param_since_2016.1_after_method_since_2015.2 -->

**Returns:** `serverWidget.Sublist`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getField(options)
<!-- id: N/ui/serverWidget.Assistant#getField -->

Returns a field on the assistant page by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.Field`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getFieldGroup(options)
<!-- id: N/ui/serverWidget.Assistant#getFieldGroup -->

Returns a field group on the assistant page by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.FieldGroup`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getFieldGroupIds()
<!-- id: N/ui/serverWidget.Assistant#getFieldGroupIds -->

No parameters.

**Returns:** `string[]`, the IDs of all field groups.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getFieldIds()
<!-- id: N/ui/serverWidget.Assistant#getFieldIds -->

No parameters.

**Returns:** `string[]`, the IDs of all fields.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getFieldIdsByFieldGroup(fieldGroup)
<!-- id: N/ui/serverWidget.Assistant#getFieldIdsByFieldGroup -->

Lists the fields inside one field group. The argument is a plain string, not an options object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `fieldGroup` | string | Yes | |

**Returns:** `string[]`, the IDs of the fields in that group.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2016.1

#### Assistant.getLastAction()
<!-- id: N/ui/serverWidget.Assistant#getLastAction -->

Tells you what the user last did (a
[`serverWidget.AssistantSubmitAction`](#serverwidgetassistantsubmitaction) value). To learn which
step that action was taken on, call [`getLastStep`](#assistantgetlaststep). No parameters.

**Returns:** `string`, the last submitted action.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getLastStep()
<!-- id: N/ui/serverWidget.Assistant#getLastStep -->

No parameters.

**Returns:** `serverWidget.AssistantStep`, the step on which the last action was submitted.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getNextStep()
<!-- id: N/ui/serverWidget.Assistant#getNextStep -->

Works out which step should follow, given the last action. Call
[`getLastStep`](#assistantgetlaststep) before this one. No parameters.

**Returns:** `serverWidget.AssistantStep`, the next step for the last action.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getStep(options)
<!-- id: N/ui/serverWidget.Assistant#getStep -->

Returns a step by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.AssistantStep`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getStepCount()
<!-- id: N/ui/serverWidget.Assistant#getStepCount -->

No parameters.

**Returns:** `number`, the total number of steps.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getSteps()
<!-- id: N/ui/serverWidget.Assistant#getSteps -->

No parameters.

**Returns:** `serverWidget.AssistantStep[]`, every step.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getSublist(options)
<!-- id: N/ui/serverWidget.Assistant#getSublist -->

Returns a sublist by ID.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `serverWidget.Sublist`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.getSublistIds()
<!-- id: N/ui/serverWidget.Assistant#getSublistIds -->

No parameters.

**Returns:** `string[]`, the IDs of all sublists.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.hasErrorHtml()
<!-- id: N/ui/serverWidget.Assistant#hasErrorHtml -->

No parameters.

**Returns:** `Boolean`, `true` when [`errorHtml`](#assistanterrorhtml) has been set.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.isFinished()
<!-- id: N/ui/serverWidget.Assistant#isFinished -->

Reports whether all steps are done; when the assistant is finished, the completion message in
[`finishedHtml`](#assistantfinishedhtml) is displayed. No parameters.

**Returns:** `Boolean`, `true` when every step is complete.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.sendRedirect(options)
<!-- id: N/ui/serverWidget.Assistant#sendRedirect -->

Handles the redirects an assistant needs, for example sending the user back to the first page of
the assistant after they cancel or finish.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.response` | response | Yes | |

- `options.response`: the source gives this type only as `response` and does not state which
  module's object it is.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.setSplash(options)
<!-- id: N/ui/serverWidget.Assistant#setSplash -->

Sets up a splash screen message for the assistant.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.text2` | string | No | |
| `options.title` | string | Yes | |
| `options.text1` | string | Yes | |

- `options.text2`: text for a second column.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### Assistant.updateDefaultValues(values)
<!-- id: N/ui/serverWidget.Assistant#updateDefaultValues -->

Sets the default values of fields on the assistant. The argument is passed directly, not inside an
`options` object.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `values` | Object[] | Yes | |

- `values`: pairs of field name and value. Listed since 2015.2.

> [!SOURCE-ERROR] The parameter is dated 2015.2, earlier than the method itself (2016.1). The source
> does not say which date is right; assume the method is available from 2016.1.
<!-- source-error: N/ui/serverWidget.Assistant#updateDefaultValues | - | param_since_2015.2_predates_method_since_2016.1 -->

**Returns:** `void`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2016.1

#### Assistant.clientScriptFileId
<!-- id: N/ui/serverWidget.Assistant#clientScriptFileId -->

`number`. File Cabinet ID of a client script for the assistant. Mutually exclusive with
[`clientScriptModulePath`](#assistantclientscriptmodulepath): setting it when the module path is
already set throws `PROPERTY_VALUE_CONFLICT`. Since 2015.2.

#### Assistant.clientScriptModulePath
<!-- id: N/ui/serverWidget.Assistant#clientScriptModulePath -->

`string`. Relative path of a client script module for the assistant. Mutually exclusive with
[`clientScriptFileId`](#assistantclientscriptfileid); setting it while the other is set throws
`PROPERTY_VALUE_CONFLICT`. Since 2016.2.

#### Assistant.currentStep
<!-- id: N/ui/serverWidget.Assistant#currentStep -->

`serverWidget.AssistantStep`, listed as read-only. The step being shown. The descriptive text says
you can set it to any step.

> [!SOURCE-ERROR] The source's type row marks this property read-only, while its description says
> it can be set to any step. The spec does not resolve this; test assignment in your account
> before relying on it to jump between steps.
<!-- source-error: N/ui/serverWidget.Assistant#currentStep | - | type_row_read_only_but_text_says_settable -->

Since 2015.2.

#### Assistant.errorHtml
<!-- id: N/ui/serverWidget.Assistant#errorHtml -->

`string`. Error message for the current step; HTML formatting is allowed. Since 2015.2.

#### Assistant.finishedHtml
<!-- id: N/ui/serverWidget.Assistant#finishedHtml -->

`string`. The completion message, displayed once [`isFinished()`](#assistantisfinished) is called.
Since 2015.2.

#### Assistant.hideAddToShortcutsLink
<!-- id: N/ui/serverWidget.Assistant#hideAddToShortcutsLink -->

`Boolean`. `true` hides the add-to-shortcuts link in the top-right corner. Default `false`.
Since 2015.2.

#### Assistant.hideStepNumber
<!-- id: N/ui/serverWidget.Assistant#hideStepNumber -->

`Boolean`. `true` drops the numbering from steps. Default `false`. Since 2015.2.

#### Assistant.isNotOrdered
<!-- id: N/ui/serverWidget.Assistant#isNotOrdered -->

`Boolean`. `true` lets the user do steps in any order, listed horizontally below the title;
`false` (the default) requires them in sequence, listed vertically in the left panel. Since 2015.2.

#### Assistant.title
<!-- id: N/ui/serverWidget.Assistant#title -->

`string`. Title shown on every page; setting it replaces the title passed to `createAssistant`.
Since 2015.2.

### AssistantStep (object)
<!-- id: N/ui/serverWidget.AssistantStep -->

One step of an assistant, created by [`Assistant.addStep`](#assistantaddstepoptions). Its getters
read what the user submitted on that step. Suitelet only, server side, since 2015.2.

#### AssistantStep.getFieldIds()
<!-- id: N/ui/serverWidget.AssistantStep#getFieldIds -->

No parameters.

**Returns:** `string[]`, the IDs of the fields in this step.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### AssistantStep.getLineCount(options)
<!-- id: N/ui/serverWidget.AssistantStep#getLineCount -->

Counts the lines of a sublist on this step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.group` | string | Yes | |

- `options.group`: the sublist ID.

**Returns:** `number`, the line count, or `-1` when the sublist does not exist. The spec also tags
the result as 0-based, which fits line numbering rather than a count and looks inconsistent.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### AssistantStep.getSublistFieldIds(options)
<!-- id: N/ui/serverWidget.AssistantStep#getSublistFieldIds -->

Lists the fields of a sublist in this step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.group` | string | Yes | |

**Returns:** `string[]`, the sublist field IDs.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### AssistantStep.getSublistValue(options)
<!-- id: N/ui/serverWidget.AssistantStep#getSublistValue -->

Reads the current value of a sublist field on one line.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.line` | number | Yes | |
| `options.group` | string | Yes | |
| `options.id` | string | Yes | |

- `options.line`: counts from 0.

**Returns:** `string`, the current value.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### AssistantStep.getSubmittedSublistIds()
<!-- id: N/ui/serverWidget.AssistantStep#getSubmittedSublistIds -->

No parameters.

**Returns:** `string[]`, the IDs of the sublists submitted on this step.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### AssistantStep.getValue(options)
<!-- id: N/ui/serverWidget.AssistantStep#getValue -->

Reads the current value of a field on this step.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string | Yes | |

**Returns:** `string | string[]`: the field's value, or an array of values for a multi-select.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### AssistantStep.helpText
<!-- id: N/ui/serverWidget.AssistantStep#helpText -->

`string`. Help text for the step. Since 2015.2.

#### AssistantStep.id
<!-- id: N/ui/serverWidget.AssistantStep#id -->

`string`, read-only. The step's internal ID. Since 2015.2.

#### AssistantStep.label
<!-- id: N/ui/serverWidget.AssistantStep#label -->

`string`. The step's label, which you can also give when creating the step with
[`addStep`](#assistantaddstepoptions). Since 2015.2.

#### AssistantStep.stepNumber
<!-- id: N/ui/serverWidget.AssistantStep#stepNumber -->

`number`. The step's position in the sequence, starting at 1. Since 2015.2.

### List (object)
<!-- id: N/ui/serverWidget.List -->

A standalone list page, created by [`serverWidget.createList`](#serverwidgetcreatelistoptions).
Suitelet only, server side, since 2015.2.

#### List.addButton(options)
<!-- id: N/ui/serverWidget.List#addButton -->

Adds a button to the list page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.functionName` | string | No | |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.functionName`: a function in the custom-module client script attached through
  `clientScriptModulePath` or `clientScriptFileId`. It cannot be the name of a client script entry
  point.
- `options.id`: lowercase, no spaces; on an existing page it needs the `custpage` prefix (e.g.
  `custpage_updateorder` for an "Update Order" button).

**Returns:** `serverWidget.Button`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### List.addColumn(options)
<!-- id: N/ui/serverWidget.List#addColumn -->

Adds a column to the list.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.align` | string | No | `LEFT` |
| `options.id` | string | Yes | |
| `options.label` | string | Yes | |

- `options.type`: a [`serverWidget.FieldType`](#serverwidgetfieldtype) value, but not every type
  works in a list column (the enum section lists the unsupported ones).
- `options.align`: a [`serverWidget.LayoutJustification`](#serverwidgetlayoutjustification) value.
- `options.id`: lowercase, no spaces.

**Returns:** `serverWidget.ListColumn`, the new column.

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### List.addEditColumn(options)
<!-- id: N/ui/serverWidget.List#addEditColumn -->

Adds a column of "Edit" (or "Edit | View") links to the left of an existing column. Works for
Suitelet lists and portlet lists; compare [`Portlet.addEditColumn`](../script-types/Portlet.md). A
generated link has the form `<link>?<linkParamName>=<row value>`; the source's illustration is an
employee record link carrying `id=123`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.showView` | Boolean | No | |
| `options.link` | string | No | |
| `options.linkParam` | string | No | |
| `options.linkParamName` | string | No | `id` |
| `options.showHrefCol` | Boolean | No | |
| `options.column` | serverWidget.ListColumn | Yes | |

- `options.showView`: `true` produces "Edit | View" links instead of "Edit" only.
- `options.link`: base URL of the links. Since 2019.2.
- `options.linkParam`: which row value goes into the link; by default the value of
  `options.column`. Using the internal ID is recommended. Since 2019.2.
- `options.linkParamName`: name of the URL parameter. Since 2019.2.
- `options.showHrefCol`: `true` makes the link URL clickable.
- `options.column`: the column the edit links are placed to the left of.

**Returns:** `serverWidget.ListColumn`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### List.addPageLink(options)
<!-- id: N/ui/serverWidget.List#addPageLink -->

Adds a link to the list page.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.title` | string | Yes | |
| `options.url` | string | Yes | |

- `options.type`: a [`serverWidget.FormPageLinkType`](#serverwidgetformpagelinktype) value.

**Returns:** `serverWidget.List`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### List.addRow(options)
<!-- id: N/ui/serverWidget.List#addRow -->

Adds one row.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.row` | Object[] | Yes | |

- `options.row`: either `search.Result` objects or name/value pairs whose names match the column
  IDs.

> [!SOURCE-ERROR] The source types `row` as an array, but its syntax sample passes a single row
> object, and the method adds only one row. The spec does not settle it; for several rows use
> [`addRows`](#listaddrowsoptions), and test what `addRow` accepts in your account.
<!-- source-error: N/ui/serverWidget.List#addRow | options.row | row_type_array_vs_single_row_object_syntax -->

**Returns:** `serverWidget.List`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### List.addRows(options)
<!-- id: N/ui/serverWidget.List#addRows -->

Adds several rows at once.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.rows` | Object[] | Yes | |

- `options.rows`: an array of `search.Result` objects, or an array of name/value-pair objects keyed
  by column ID.

**Returns:** `serverWidget.List`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2015.2

#### List.clientScriptFileId
<!-- id: N/ui/serverWidget.List#clientScriptFileId -->

`number`. File Cabinet ID of a client script for the list. Mutually exclusive with
[`clientScriptModulePath`](#listclientscriptmodulepath); setting one while the other is set throws
`PROPERTY_VALUE_CONFLICT`. Since 2016.1.

#### List.clientScriptModulePath
<!-- id: N/ui/serverWidget.List#clientScriptModulePath -->

`string`. Relative path of a client script module for the list. Mutually exclusive with
[`clientScriptFileId`](#listclientscriptfileid); setting one while the other is set throws
`PROPERTY_VALUE_CONFLICT`. Since 2016.2.

#### List.style
<!-- id: N/ui/serverWidget.List#style -->

`string`. Visual style, a [`serverWidget.ListStyle`](#serverwidgetliststyle) value. Since 2015.2.

#### List.title
<!-- id: N/ui/serverWidget.List#title -->

`string`. The list's title. Since 2015.2.

### ListColumn (object)
<!-- id: N/ui/serverWidget.ListColumn -->

One column of a [`List`](#list-object), returned by [`List.addColumn`](#listaddcolumnoptions) and
[`List.addEditColumn`](#listaddeditcolumnoptions). Suitelet only, server side, since 2015.2.

#### ListColumn.addParamToURL(options)
<!-- id: N/ui/serverWidget.ListColumn#addParamToURL -->

Appends a URL parameter to the links in this column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.dynamic` | Boolean | No | |
| `options.param` | string | Yes | |
| `options.value` | string | Yes | |

- `options.dynamic`: `true` treats `value` as an alias that is resolved for each row, rather than a
  fixed value.
- The source does not state since which version each parameter exists; the method itself is dated
  2016.1.

**Returns:** `serverWidget.ListColumn`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2016.1

#### ListColumn.setURL(options)
<!-- id: N/ui/serverWidget.ListColumn#setURL -->

Sets the base URL used for the links in this column.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.dynamic` | Boolean | No | |
| `options.url` | string | Yes | |

- `options.dynamic`: `true` treats `url` as an alias resolved per row.
- The source does not state since which version each parameter exists; the method itself is dated
  2016.1.

**Returns:** `serverWidget.ListColumn`

**Governance:** 0 units.

**Availability:** server; Suitelet.

**Since:** 2016.1

#### ListColumn.label
<!-- id: N/ui/serverWidget.ListColumn#label -->

`string`. The column heading. Since 2016.1.

## Enums

### serverWidget.AssistantSubmitAction
<!-- id: N/ui/serverWidget.AssistantSubmitAction -->

The values [`Assistant.getLastAction()`](#assistantgetlastaction) can return. Since 2015.2. The
source describes only `FINISH` and `JUMP`; it does not state the meaning of the other values beyond
their names.

| Value | Meaning |
|---|---|
| `BACK` | Not stated in the source |
| `CANCEL` | Not stated in the source |
| `FINISH` | Unless you change it, the finish page shows a congratulations text |
| `JUMP` | In an assistant whose steps are not ordered, a move to the last action (as the source phrases it) |
| `NEXT` | Not stated in the source |

### serverWidget.FieldBreakType
<!-- id: N/ui/serverWidget.FieldBreakType -->

Used by [`Field.updateBreakType`](#fieldupdatebreaktypeoptions). Since 2015.2.

| Value | Meaning |
|---|---|
| `NONE` | No break; the default |
| `STARTCOL` | Starts a new column and turns off automatic balancing of fields between columns |
| `STARTROW` | Starts a new row; for fields placed outside the normal layout |

### serverWidget.FieldDisplayType
<!-- id: N/ui/serverWidget.FieldDisplayType -->

Used by [`Field.updateDisplayType`](#fieldupdatedisplaytypeoptions). A field set to `NODISPLAY`
can later be made visible by a client script. Since 2015.2.

| Value | Meaning |
|---|---|
| `DISABLED` | Shown, but the user cannot change it |
| `ENTRY` | Entry input inside a sublist |
| `HIDDEN` | Hidden |
| `INLINE` | Shown as inline text |
| `NODISPLAY` | Not shown, but can be made visible later |
| `NORMAL` | Normal input (outside sublists) |
| `READONLY` | Disabled, but its text can still be selected (text areas) |

### serverWidget.FieldLayoutType
<!-- id: N/ui/serverWidget.FieldLayoutType -->

Used by [`Field.updateLayoutType`](#fieldupdatelayouttypeoptions). Since 2015.2.

| Value | Meaning |
|---|---|
| `STARTROW` | First field of a horizontal group |
| `MIDROW` | A middle field of a horizontal group |
| `ENDROW` | Last field of a horizontal group |
| `OUTSIDE` | Outside the normal layout, placed according to the form's default |
| `OUTSIDEBELOW` | Below the normal layout area |
| `OUTSIDEABOVE` | Above the normal layout area |
| `NORMAL` | The default position |

### serverWidget.FieldType
<!-- id: N/ui/serverWidget.FieldType -->

Field data types. Every method that creates a field or column takes its `options.type` from this
enum: [`Form.addField`](#formaddfieldoptions), [`Sublist.addField`](#sublistaddfieldoptions),
[`Assistant.addField`](#assistantaddfieldoptions) and [`List.addColumn`](#listaddcolumnoptions).
Since 2015.2.

Rules from the spec:

- Long text fields created by script hold up to 100,000 characters; SuiteBuilder long text fields
  allow 1,000,000.
- `DATETIME` is not supported by the `addField` methods; use `DATETIMETZ` for a date/time field.
- `FILE` fields only work on the main tab of a Suitelet; they cannot go on tabs, sublists, field
  groups or existing pages.
- `IMAGE` fields are only available on lists, static list sublists and forms.
- `INLINEHTML` is write-only and hides its label; add a separate field if you need a caption. Only
  `INLINEHTML` accepts HTML; every other type takes plain text.
- `RADIO` buttons are mutually exclusive within their container.
- `EMAIL` and `PHONE` values get a basic format check.
- Not supported by `Sublist.addField`: `DATETIME`, `FILE`, `HELP`, `INLINEHTML`, `LABEL`,
  `LONGTEXT`, `MULTISELECT`, `RADIO`, `RICHTEXT`.
- Not supported by `List.addColumn`: `CHECKBOX`, `DATETIME`, `DATETIMETZ`, `FILE`, `HELP`,
  `INLINEHTML`, `LABEL`, `MULTISELECT`, `SELECT`, `RADIO`.

Beyond these rules, the source does not describe what each individual value means; the notes below
only restate the rules per value.

| Value | Notes |
|---|---|
| `CHECKBOX` | Not in list columns |
| `CURRENCY` | |
| `DATE` | |
| `DATETIME` | Not usable through `addField`; use `DATETIMETZ` |
| `DATETIMETZ` | Not in list columns |
| `EMAIL` | Basic format check |
| `FILE` | Suitelet main tab only; not in sublists or list columns |
| `FLOAT` | |
| `HELP` | Not in sublists or list columns |
| `INLINEHTML` | Write-only, no label; the only type that accepts HTML; not in sublists or list columns |
| `INTEGER` | |
| `IMAGE` | Lists, static list sublists and forms only |
| `LABEL` | Not in sublists or list columns |
| `LONGTEXT` | Up to 100,000 characters; not in sublists |
| `MULTISELECT` | Not in sublists or list columns |
| `PASSWORD` | |
| `PERCENT` | |
| `PHONE` | Basic format check |
| `SELECT` | Not in list columns |
| `RADIO` | Exclusive within its container; not in sublists or list columns |
| `RICHTEXT` | Not in sublists |
| `TEXT` | |
| `TEXTAREA` | |
| `TIMEOFDAY` | |
| `URL` | |

### serverWidget.FormPageLinkType
<!-- id: N/ui/serverWidget.FormPageLinkType -->

Used by [`Form.addPageLink`](#formaddpagelinkoptions) (and [`List.addPageLink`](#listaddpagelinkoptions)).
Since 2015.2.

| Value | Where the link appears |
|---|---|
| `BREADCRUMB` | Top left, after the breadcrumb trail |
| `CROSSLINK` | Top right |

### serverWidget.LayoutJustification
<!-- id: N/ui/serverWidget.LayoutJustification -->

Column alignment for [`List.addColumn`](#listaddcolumnoptions). Since 2015.2. The source gives no
per-value description beyond the names.

| Value |
|---|
| `CENTER` |
| `LEFT` |
| `RIGHT` |

### serverWidget.ListStyle
<!-- id: N/ui/serverWidget.ListStyle -->

Values for [`List.style`](#liststyle). Since 2015.2. The source does not describe what each style
looks like.

| Value |
|---|
| `GRID` |
| `NORMAL` |
| `PLAIN` |
| `REPORT` |

### serverWidget.SublistDisplayType
<!-- id: N/ui/serverWidget.SublistDisplayType -->

Values for [`Sublist.displayType`](#sublistdisplaytype). Since 2015.2. The source gives no
per-value description beyond the names.

| Value |
|---|
| `HIDDEN` |
| `NODISPLAY` |
| `NORMAL` |

### serverWidget.SublistType
<!-- id: N/ui/serverWidget.SublistType -->

Sublist styles for [`Form.addSublist`](#formaddsublistoptions). The item sublist of a sales order
and the expense sublist of an expense report are typical inline editor sublists; the `EDITOR` style
is uncommon on standard records. Lines cannot be added to a `LIST` sublist, but you can make one of
its fields editable by setting that field's display type to `ENTRY` with
[`Field.updateDisplayType`](#fieldupdatedisplaytypeoptions). Since 2015.2.

| Value | Meaning |
|---|---|
| `INLINEEDITOR` | Editable; new lines are entered directly in an empty line at the bottom |
| `EDITOR` | Editable; lines are added through a form |
| `LIST` | Fixed set of lines; individual fields can be made editable through their display type |
| `STATICLIST` | Read-only and not scriptable |

## Examples

A Suitelet where a store manager asks the warehouse for a cycle count. The GET request draws a form
with a field group, an inline editor sublist and a submit button; the POST request reads the
submitted lines and logs a task for the warehouse team. The sublist columns are added without a
`container`, since what that option does for sublist fields is not settled.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/record'], (serverWidget, record) => {
  const LINES = 'custpage_count_lines';

  const buildRequestForm = () => {
    const form = serverWidget.createForm({ title: 'Cycle Count Request' });

    form.addFieldGroup({ id: 'custpage_grp_request', label: 'Request details' });

    const warehouse = form.addField({
      id: 'custpage_location',
      type: serverWidget.FieldType.SELECT,
      label: 'Warehouse',
      source: 'location',
      container: 'custpage_grp_request'
    });
    warehouse.isMandatory = true;

    form.addField({
      id: 'custpage_reason',
      type: serverWidget.FieldType.TEXTAREA,
      label: 'Why is a count needed?',
      container: 'custpage_grp_request'
    }).setHelpText({ help: 'Mention the bin or aisle if the discrepancy is local.' });

    const lines = form.addSublist({
      id: LINES,
      type: serverWidget.SublistType.INLINEEDITOR,
      label: 'Items to count'
    });
    lines.addField({ id: 'custpage_item', type: serverWidget.FieldType.SELECT, label: 'Item', source: 'item' });
    lines.addField({ id: 'custpage_expected', type: serverWidget.FieldType.INTEGER, label: 'Expected on hand' });

    form.addSubmitButton({ label: 'Send to warehouse' });
    return form;
  };

  const logCountTask = (request) => {
    const lineCount = request.getLineCount({ group: LINES });
    const notes = [];
    for (let line = 0; line < lineCount; line += 1) {
      const item = request.getSublistValue({ group: LINES, name: 'custpage_item', line });
      const expected = request.getSublistValue({ group: LINES, name: 'custpage_expected', line });
      notes.push(`item ${item}: expected ${expected}`);
    }

    const task = record.create({ type: record.Type.TASK });
    task.setValue({ fieldId: 'title', value: `Cycle count, location ${request.parameters.custpage_location}` });
    task.setValue({ fieldId: 'message', value: `${request.parameters.custpage_reason || ''}\n${notes.join('\n')}` });
    return task.save();
  };

  const onRequest = (context) => {
    if (context.request.method === 'GET') {
      context.response.writePage({ pageObject: buildRequestForm() });
      return;
    }

    const taskId = logCountTask(context.request);
    const done = serverWidget.createForm({ title: 'Request sent' });
    const summary = done.addField({
      id: 'custpage_result',
      type: serverWidget.FieldType.TEXT,
      label: 'Warehouse task'
    });
    summary.defaultValue = String(taskId);
    summary.updateDisplayType({ displayType: serverWidget.FieldDisplayType.INLINE });
    context.response.writePage({ pageObject: done });
  };

  return { onRequest };
});
```

A user event that, when a sales order is viewed, adds a "Dispatch" tab with two read-only fields
and a button wired to a function in a client script module. The tab gets two fields because a tab
with fewer than two fields shows them at the bottom of the form instead.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/ui/serverWidget'], (serverWidget) => {
  const beforeLoad = (context) => {
    if (context.type !== context.UserEventType.VIEW) {
      return;
    }

    const form = context.form;
    const order = context.newRecord;
    form.clientScriptModulePath = './so_dispatch_client.js';

    form.addTab({ id: 'custpage_dispatch', label: 'Dispatch' });

    const instructions = form.addField({
      id: 'custpage_pack_notes',
      type: serverWidget.FieldType.TEXTAREA,
      label: 'Packing instructions',
      container: 'custpage_dispatch'
    });
    instructions.defaultValue = order.getValue({ fieldId: 'memo' }) || 'None recorded';
    instructions.updateDisplayType({ displayType: serverWidget.FieldDisplayType.INLINE });

    const shipBy = form.addField({
      id: 'custpage_ship_by',
      type: serverWidget.FieldType.TEXT,
      label: 'Ship by',
      container: 'custpage_dispatch'
    });
    shipBy.defaultValue = String(order.getValue({ fieldId: 'shipdate' }) || 'Not scheduled');
    shipBy.updateDisplayType({ displayType: serverWidget.FieldDisplayType.INLINE });

    form.addButton({
      id: 'custpage_notify_dock',
      label: 'Notify loading dock',
      functionName: 'notifyLoadingDock'
    });
  };

  return { beforeLoad };
});
```

A standalone list of the day's dock appointments, built from plain name/value rows keyed by column
ID:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget'], (serverWidget) => {
  const onRequest = (context) => {
    const list = serverWidget.createList({ title: 'Dock Appointments Today' });
    list.style = serverWidget.ListStyle.REPORT;

    list.addColumn({ id: 'carrier', type: serverWidget.FieldType.TEXT, label: 'Carrier' });
    list.addColumn({ id: 'slot', type: serverWidget.FieldType.TEXT, label: 'Time slot' });
    list.addColumn({
      id: 'pallets',
      type: serverWidget.FieldType.INTEGER,
      label: 'Pallets',
      align: serverWidget.LayoutJustification.RIGHT
    });

    list.addRows({
      rows: [
        { carrier: 'Northline Freight', slot: '08:00-09:00', pallets: '12' },
        { carrier: 'Harbor Express', slot: '10:30-11:00', pallets: '4' }
      ]
    });

    context.response.writePage({ pageObject: list });
  };

  return { onRequest };
});
```
