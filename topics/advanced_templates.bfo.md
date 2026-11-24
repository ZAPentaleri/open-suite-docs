# Advanced template markup and BFO
<!-- id: topic:advanced_templates.bfo -->

Part of: [Advanced PDF/HTML templates](advanced_templates.md)

See also: [Advanced template field syntax](advanced_templates.field_syntax.md),
[Advanced template fonts, languages and bar codes](advanced_templates.localization_and_barcodes.md).

An advanced template is an XML document that mixes FreeMarker instructions with BFO Report
Generator elements. FreeMarker fills in the data; BFO then lays the resulting XML out as a PDF.

## Document skeleton

Every template must start with these three items:

1. Line 1: the XML declaration, with XML version `1.0`.
2. Line 2: the BFO report DOCTYPE. Its parts, exactly as the source gives them:
   - root element: `pdf`
   - public identifier: `-//big.faceless.org//report`
   - system identifier: `report-1.1.dtd`

   Put together as an XML DOCTYPE declaration, the line reads
   `<!DOCTYPE pdf PUBLIC "-//big.faceless.org//report" "report-1.1.dtd">`.
3. The `pdf` root element, which encloses the rest of the document and is closed at its end.

> [!SOURCE-ERROR] The source's text says the closing `pdf` tag is on line 8 of its skeleton example,
> but in that example it is on line 9. The line number does not matter; what matters is that `</pdf>`
> is the last element and closes the document.
<!-- source-error: topic:advanced_templates.bfo | - | text_cites_line_8_for_pdf_close_example_line_9 -->

Further rules for the document:

- It must be well-formed XML: close every element and quote every attribute value. Email, fax and
  marketing templates must be valid XML too.
- HTML5 declarations are not supported. CSS2 may be used.
- Treat any content outside the `head` and `body` sections as a likely source of trouble.
- Avoid `tbody`; a template that contains it may fail to save.
- When the document is printed as HTML, the PDF-specific tags are converted to HTML.

## BFO elements

| Element | Purpose |
|---|---|
| `pagenumber` | The current page number |
| `totalpages` | The total number of pages |
| `macrolist`, `macro` | Containers for reusable header, footer and background blocks |
| `pbr` | A page break |
| `barcode` | A bar code (see [fonts, languages and bar codes](advanced_templates.localization_and_barcodes.md#bar-codes)) |
| `pdfset` | Combines several PDFs into one output |

Page numbers are filled in by BFO during layout, after FreeMarker has finished, so they cannot be
used inside FreeMarker expressions.

## Headers, footers and backgrounds

- There are three kinds of macro: header, footer and background. They are defined in the macro list
  and selected through attributes of `body`: `header`, `header-height`, `footer`, `footer-height`
  and `background-macro`.
- A header or footer macro needs its height attribute as well.
- A template cannot define several headers. A page break (`pbr`) lets you change the macros used
  from the next page onward.

## Page breaks and long content

- These elements can flow across a page break: `table`, `ul`, `p`, `pre`, `ol`, `h1`, `h2`, `h3`,
  `h4` and `blockquote`.
- A table placed inside a `td` does not flow; it is cut off where the page ends.
- A row holding text longer than 2,000 characters may be truncated. Split such text into extra rows.

## Tables and spacing

- Format tables through CSS.
- Size table cells with explicit `width` and `height`.
- The non-breaking space entity is not rendered. Make space with `br` elements or with cell sizes.
- For striped rows, take the item index modulo 2 and pick the row style from the result.

## Attachments and appended PDFs

- To attach a file to the generated PDF, put a `link` element of type attachment carrying the file
  data in the `head` section.
- To append a second PDF, use `pdfset` with that PDF as its `src`.
- Files used this way must be available without login.
- A link source can be a File Cabinet URL or a field value read with the URL suffix (`@url`).
- Escape ampersands in link URLs.

## Errors and validation

- Errors come from two sources: the virtual record that is evaluated when the template is saved, and
  the preview, save or print itself. Error messages give the line, the column and the template name.
- The virtual record used at save time holds short strings, so an expression such as a substring
  call can fail against it. Check the string's length before cutting it.
- The template is validated when you switch to the WYSIWYG editor and when you save.
- The XML input for PDF creation may be at most 100 MB.

## Example

A skeleton for a receiving checklist, written for this page: header and footer macros, a page
counter, and striped rows.

```ftl
<?xml version="1.0"?>
<!DOCTYPE pdf PUBLIC "-//big.faceless.org//report" "report-1.1.dtd">
<pdf>
<head>
  <macrolist>
    <macro id="rcv_head"><p>Receiving checklist for ${record.tranid}</p></macro>
    <macro id="rcv_foot"><p align="right"><pagenumber/> / <totalpages/></p></macro>
  </macrolist>
  <style type="text/css">
    td.even { background-color: #f3f6ee; }
    td.qty { width: 25mm; }
  </style>
</head>
<body header="rcv_head" header-height="14mm" footer="rcv_foot" footer-height="8mm">
  <table>
  <#list record.item as line>
    <#if line?index % 2 == 0><#assign rowClass = "even"><#else><#assign rowClass = "odd"></#if>
    <tr>
      <td class="${rowClass}" width="120mm">${line.item}</td>
      <td class="${rowClass} qty" align="right">${line.quantity}</td>
    </tr>
  </#list>
  </table>
</body>
</pdf>
```
