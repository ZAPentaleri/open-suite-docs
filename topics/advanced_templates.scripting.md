# Scripting advanced templates
<!-- id: topic:advanced_templates.scripting -->

Part of: [Advanced PDF/HTML templates](advanced_templates.md)

See also: [N/render](../modules/N_render.md), [Suitelet](../script-types/Suitelet.md),
[Advanced template markup and BFO](advanced_templates.bfo.md).

Scripts use [N/render](../modules/N_render.md) to print with advanced templates. This works in
server scripts only. Scripts cannot touch the user interface's DOM, so everything goes through the
render API.

## Printing with the form's own template

These functions print with the advanced template assigned to the record's custom form:

- [`render.bom`](../modules/N_render.md)
- [`render.packingSlip`](../modules/N_render.md)
- [`render.pickingTicket`](../modules/N_render.md)
- [`render.statement`](../modules/N_render.md)
- [`render.transaction`](../modules/N_render.md)

Non-transaction records have no such shortcut; printing them requires SuiteScript with a
[`TemplateRenderer`](../modules/N_render.md).

## Choosing a template for a TemplateRenderer

- Prefer [`TemplateRenderer.setTemplateByScriptId()`](../modules/N_render.md); it is the
  recommended way to pick a template.
- Use [`TemplateRenderer.setTemplateById()`](../modules/N_render.md) (internal ID) for custom print
  solutions that need it.
- You can also hand the renderer template source as a string through
  [`TemplateRenderer.templateContent`](../modules/N_render.md). The template then lives in the script
  and end users never see it.
- A template rendered from a script still starts with a `pdf` root and the BFO doctype (see
  [markup](advanced_templates.bfo.md)).
- A custom field sourced from the Advanced PDF/HTML Templates list shows every template, unfiltered.

> [!SOURCE-ERROR] The source's sample template has a malformed attribute on its `img` element. Make
> sure every attribute in your template is a properly quoted name-value pair; BFO rejects anything
> else.
<!-- source-error: topic:advanced_templates.scripting | - | sample_template_has_malformed_img_attribute -->

## Feeding data to a template

- Several records can go into one template, each under its own alias (see
  [`TemplateRenderer.addRecord()`](../modules/N_render.md)).
- A plain JSON-style object can serve as a data source, including collections that the template
  loops over.
- One template can combine JSON, XML, search result and record sources.

> [!SOURCE-ERROR] The source's sample Suitelet calls the record module without listing `N/record` in
> its `define` dependencies, so it would fail at run time. Load every module you call through
> `define`.
<!-- source-error: topic:advanced_templates.scripting | - | sample_suitelet_uses_record_module_not_imported -->

### Custom data sources

[`TemplateRenderer.addCustomDataSource()`](../modules/N_render.md) accepts data in the formats of the
[`render.DataSource`](../modules/N_render.md) enum: `XML_DOC`, `XML_STRING`, `OBJECT` and `JSON`.

- Wrap each template section that reads a custom source in a `has_content` check, so the template
  still renders when the data is missing.
- Custom sources are not visible in the template editor or its preview.
- Every value from a custom source arrives as a string; convert in the template if you need numbers
  or dates.
- JSON is converted to XML internally, so key names must be valid XML names; in particular they
  cannot start with a digit.

> [!SOURCE-ERROR] The source labels a FreeMarker snippet as a JavaScript file. Template code belongs
> in the template, not in the script file.
<!-- source-error: topic:advanced_templates.scripting | - | freemarker_snippet_called_javascript_file -->

> [!SOURCE-ERROR] One source example calls `getRange` on the search object itself, with positional
> arguments. In SuiteScript 2.x, run the search first and page through the result set using an
> options object; check [N/search](../modules/N_search.md) for the exact calls.
<!-- source-error: topic:advanced_templates.scripting | - | example_calls_getrange_on_search_object -->

## Producing the PDF

[`render.xmlToPdf()`](../modules/N_render.md) turns rendered XML into a PDF file. It takes an options
object whose `xmlString` holds the XML.

> [!SOURCE-ERROR] The source's sample passes the XML string straight to `xmlToPdf` instead of an
> options object. Pass `{ xmlString: ... }`.
<!-- source-error: topic:advanced_templates.scripting | - | sample_passes_string_to_xmltopdf_not_options -->

## Bulk printing

To print many records in one PDF:

1. Run a saved search and collect results in batches of 1,000.
2. Use a template built around `pdfset`, one PDF per record.
3. Drive it from a Suitelet.

If the template shows currency symbols, the search needs a currency column.

> [!SOURCE-ERROR] The source's bulk-printing steps name SuiteScript 1.0 functions (such as
> `nlapiSearchRecord`) in a 2.x topic. Use the N/search and N/render equivalents instead.
<!-- source-error: topic:advanced_templates.scripting | - | bulk_printing_steps_name_1_0_functions_in_2_x_topic -->

## Example

A Suitelet that prints a purchase order with a template chosen by script ID and adds a small
object data source for a receiving-dock note. The template script ID and the `poid` request
parameter are invented.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/render', 'N/record'], (render, record) => {
  const onRequest = (context) => {
    const poId = context.request.parameters.poid;
    const po = record.load({ type: record.Type.PURCHASE_ORDER, id: poId });

    const renderer = render.create();
    renderer.setTemplateByScriptId({ scriptId: 'custtmpl_po_dock_copy' });
    renderer.addRecord({ templateName: 'record', record: po });
    renderer.addCustomDataSource({
      format: render.DataSource.OBJECT,
      alias: 'dock',
      data: { bay: 'B4', window: 'morning' }
    });

    renderer.renderPdfToResponse({ response: context.response });
  };

  return { onRequest };
});
```

In the template, guard the custom section: `<#if dock?has_content>Bay ${dock.bay}</#if>`.
