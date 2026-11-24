# SuiteScript 2.1 custom pages
<!-- id: topic:custom_pages -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

Sub-topics:

- [Custom assistants](custom_pages.assistants.md)

See also: [N/ui/serverWidget](../modules/N_ui_serverWidget.md), [N/ui/message](../modules/N_ui_message.md),
[N/ui/dialog](../modules/N_ui_dialog.md), [Suitelet](../script-types/Suitelet.md),
[Portlet](../script-types/Portlet.md), [UserEventScript](../script-types/UserEventScript.md).

This topic is about building your own user interface in NetSuite: custom pages and the components they
are made of, the UI modules, pages written in plain HTML, and assistants.

## Three ways to build UI

You can customize forms point-and-click, build pages from N/ui/serverWidget components, or serve your
own HTML from a Suitelet. Each trades ease against control:

| Approach | Strengths | Limits |
|---|---|---|
| Point-and-click customization | Easy | Less flexible; it can only change or copy forms that already exist |
| [N/ui/serverWidget](../modules/N_ui_serverWidget.md) components | Flexible and precise, for example a layout that has to line up with preprinted paper; scripts can also automate validations, calculations and record updates | Cannot start from existing form templates |
| HTML in a Suitelet | Fits brand or design needs | Handling the submitted form data is harder |

Whichever approach you use, scripts have no direct access to the DOM.

## Script types and page types

UI is built on the server by [workflow action scripts](../script-types/WorkflowActionScript.md),
[Suitelets](../script-types/Suitelet.md), [portlets](../script-types/Portlet.md) and
[user event scripts](../script-types/UserEventScript.md). Of these, Suitelets give you the most
freedom.

There are three kinds of page, each an object in N/ui/serverWidget:

| Page | Object | Built by |
|---|---|---|
| Form | [`serverWidget.Form`](../modules/N_ui_serverWidget.md) | Suitelets, portlets, and user event scripts in `beforeLoad` |
| List | [`serverWidget.List`](../modules/N_ui_serverWidget.md) | Suitelets and portlets |
| Assistant | [`serverWidget.Assistant`](../modules/N_ui_serverWidget.md) | see [Custom assistants](custom_pages.assistants.md) |

- Portlets appear only on dashboards.
- To change an existing form when a record is opened for editing, use a user event script's
  `beforeLoad` entry point.
- Every component is generated on the server. Client scripts can still reach those server-generated
  components.

## Forms

A form is assembled from buttons, fields, field groups, tabs and subtabs, sublists, and page links;
these are the same standard button, field, tab and sublist components NetSuite pages use. You add them
with methods such as [`Form.addButton()`](../modules/N_ui_serverWidget.md),
[`Form.addSubmitButton()`](../modules/N_ui_serverWidget.md) and
[`Form.addField()`](../modules/N_ui_serverWidget.md). Field types come from the
[`serverWidget.FieldType`](../modules/N_ui_serverWidget.md) enum. A sublist can display the results of
a saved search.

### Field layout

- The allowed position values are in the field layout type and field break type enums of
  N/ui/serverWidget. The break type defaults to `NONE`.
- A start-column break has no effect inside a field group that has only one column.
- A start-row break works only on fields that are not inside a field group.
- Forms do not support collapsing or hiding field groups.

### Page links

Forms can carry breadcrumb links and crosslinks. The kind is picked with the
[`serverWidget.FormPageLinkType`](../modules/N_ui_serverWidget.md) enum.

### Credential fields

A credential field keeps its value encrypted, and its use can be limited by domain, by script and by
user. It is meant to hold a secret key for [N/crypto](../modules/N_crypto.md).

### Inline HTML

To place HTML on a form, add a field of type `INLINEHTML` with `Form.addField()` and put the markup in
the field's [`Field.defaultValue`](../modules/N_ui_serverWidget.md). HTML input elements are allowed
only inside inline HTML fields. Inline HTML on forms that are not Suitelet forms is planned for
deprecation.

## Lists

A list is made of buttons, columns, page links and rows, added with
[`List.addButton()`](../modules/N_ui_serverWidget.md), [`List.addColumn()`](../modules/N_ui_serverWidget.md),
[`List.addEditColumn()`](../modules/N_ui_serverWidget.md), [`List.addPageLink()`](../modules/N_ui_serverWidget.md),
[`List.addRow()`](../modules/N_ui_serverWidget.md) and [`List.addRows()`](../modules/N_ui_serverWidget.md).
To turn a column's values into links, use [`ListColumn.setURL()`](../modules/N_ui_serverWidget.md) and
[`ListColumn.addParamToURL()`](../modules/N_ui_serverWidget.md). Page links are breadcrumbs or
crosslinks ([`serverWidget.FormPageLinkType`](../modules/N_ui_serverWidget.md)), as on forms.

## HTML pages from a Suitelet

A Suitelet can serve a plain HTML page instead of serverWidget objects. The page markup is retrieved
with [`https.get()`](../modules/N_https.md), and the Suitelet then processes whatever data the page
posts back. Unlike a serverWidget page, such a Suitelet returns a string rather than UI objects. The
source illustrates this with a volunteer sign-up sheet.

- Use an HTML templating library; the templating engine on the server side is FreeMarker.
- External content that the page pulls in must be protected with subresource integrity.

## UI modules

The modules for building UI are [N/ui/serverWidget](../modules/N_ui_serverWidget.md),
[N/ui/message](../modules/N_ui_message.md) and [N/ui/dialog](../modules/N_ui_dialog.md). Dialogs are
promise-based.

## About the source's step-by-step form sample

The source builds a custom form in numbered steps. That walkthrough has several defects, so do not copy
its fragments as they stand.

> [!SOURCE-ERROR] In the step-by-step custom form sample, the per-step snippets end the `define` call
> without its closing parenthesis, the skeleton writes the page before the form object has been
> created, and one step's text has the typo "ccustomize". Write the page only after building the form,
> and close every `define(...)` call properly, as in the example below.
<!-- source-error: topic:custom_pages | - | step_snippets_close_define_without_paren -->
<!-- source-error: topic:custom_pages | - | step_skeleton_writes_page_before_form_created -->
<!-- source-error: topic:custom_pages | - | step_text_typo_ccustomize -->

## Example

A Suitelet that shows a warranty claim intake form with an inline HTML notice, then acknowledges the
submission with a plain string response.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/log'], (serverWidget, log) => {
  const buildForm = () => {
    const form = serverWidget.createForm({ title: 'Warranty Claim Intake' });
    const notice = form.addField({
      id: 'custpage_claim_notice',
      label: 'Notice',
      type: serverWidget.FieldType.INLINEHTML
    });
    notice.defaultValue = '<p><b>Have the packing slip number ready before you submit.</b></p>';
    form.addField({
      id: 'custpage_contact_email',
      label: 'Contact email',
      type: serverWidget.FieldType.EMAIL
    });
    form.addSubmitButton({ label: 'Submit claim' });
    return form;
  };

  const onRequest = (context) => {
    if (context.request.method === 'GET') {
      context.response.writePage({ pageObject: buildForm() });
      return;
    }
    const email = context.request.parameters.custpage_contact_email;
    log.audit({ title: 'Warranty claim received', details: email });
    context.response.write({ output: 'Claim received. Our service team will contact you.' });
  };

  return { onRequest };
});
```
