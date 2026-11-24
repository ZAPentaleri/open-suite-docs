# SuiteScript Records Browser
<!-- id: topic:records_browser -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [Records Catalog](records_catalog.md), [N/record](../modules/N_record.md),
[record reference index](../gen/records/index.md).

The Records Browser is the reference that lists which records, fields, sublists, search joins, search
filters, search columns and transformations SuiteScript supports. The record pages generated for this
reference ([`out/gen/records/`](../gen/records/index.md)) hold the same kind of information. These are
the IDs used with [N/record](../modules/N_record.md) and [N/search](../modules/N_search.md); SuiteQL
table and column names come from the [Records Catalog](records_catalog.md) instead.

## What a record page shows

Each record's page is built from these tables:

- fields (the body fields);
- sublists (with their fields);
- tabs;
- search joins;
- search filters;
- search columns;
- transform types (the record types this one can be transformed into).

A record can also carry one of two designations:

- **Search only**: searching is the only thing a script can do with this record type.
- **Supports deleted record search**: the record type can be chosen as a filter when searching for
  deleted records (see below).

In the fields table, the `nlapiSubmitField` column marks the fields that can be updated with
[`record.submitFields()`](../modules/N_record.md).

Only record types that officially support SuiteScript are listed.

## Rules for using the IDs

- Use only IDs the browser documents. IDs you can spot elsewhere, such as in a page's HTML source, are
  not all supported, and undocumented IDs may change without notice.
- A script can set a field exactly when a user could set it in the UI.
- A field that is read-only in the UI can still be read by a script, for example with `getValue()`.

## Navigating

- The browser can be used online or downloaded. Only the browser for the current release is
  supported.
- Items that are new in the current release are highlighted in green.
- An A-to-Z index runs along the top. Clicking a letter lists the record types that start with it and
  opens the first one in the center pane.
- Subrecords are reached through links on the summary fields of their parent records; for example the
  sales order's `billingaddress` field links to the address subrecord.
- The browser is cross-linked with the SOAP Schema Browser and the Connect Browser: its tabs open the
  matching page in those browsers.

## Deleted record search

The deleted record entry is filed under the letter D. A search for deleted records can be filtered by
record type only for record types that have the supports-deleted-record-search designation. For
finding deleted records, the SOAP web services get-deleted operation is an alternative.
