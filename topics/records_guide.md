# SuiteScript records guide
<!-- id: topic:records_guide -->

Sub-topics:

- [Activity records](records_guide.activities.md)
- [Communication records](records_guide.communications.md)
- [Customization records](records_guide.customization.md)
- [Entity records](records_guide.entities.md)
- [File Cabinet records](records_guide.file_cabinet.md)
- [Item records](records_guide.items.md)
- [List records](records_guide.lists.md)
- [Marketing records](records_guide.marketing.md)
- [Support records](records_guide.support.md)
- [Transactions-related records](records_guide.transactions_related.md)
- [Website records](records_guide.website.md)

See also: [SuiteScript Records Browser](records_browser.md),
[record reference pages](../gen/records/index.md), [Connect tables](../gen/connect/index.md),
[N/record](../modules/N_record.md), [N/search](../modules/N_search.md).

The records guide is meant to be read alongside the [Records Browser](records_browser.md). The
browser enumerates each record type's fields, sublists, search filters and search columns; the guide
explains how scripts are allowed to work with those record types. In this reference the browser's
data is rendered mechanically under [`gen/records/`](../gen/records/index.md), and the pages in this
topic family add the behaviour that a field table cannot carry.

## What a record page tells you

Each record type's page in the guide covers four things:

- the **script types** that may work with the record,
- the **functions** (operations) a script may perform on it,
- the **field definitions**, and
- **usage notes** with caveats particular to that record.

The Records Browser also works offline. It ships as a downloadable ZIP archive: download it, extract
it, and open the `index.html` file in the script directory of the extracted tree in a browser.

## Scripting levels and subrecords

A record described as fully scriptable supports the complete set of operations: create, update,
copy, delete and search. Records at a lower scripting level support only part of that set, as their
pages say.

Subrecords can be scripted from the line level (for example from a sublist line of the parent record)
unless the record's page says otherwise.

## Support across SuiteCloud channels

The overview table of supported records has these columns: the record ID, its category, its scripting
level, and whether it can be used in client scripts and in server scripts.

A second view compares which records each integration channel supports: SuiteScript, SOAP web
services, CSV import, SuiteAnalytics Connect and REST web services. The per-record answers are in the
[SuiteCloud record support catalog](../gen/catalogs/suitecloud_record_support.md).

- The SOAP column reflects the record list of the 2020.1 SOAP endpoint.
- Connect schema information only applies when the `SuiteAnalytics Connect` feature is enabled in the
  account (see the [features catalog](../gen/catalogs/features.md)).
- A Connect table does not necessarily expose every field the matching record has. A field you can
  set from a script is not guaranteed to be readable over Connect; check the table on its
  [Connect page](../gen/connect/index.md).

## Moving customizations between accounts

SuiteBundler remains supported but gets no new features. The alternatives to plan around are Copy to
Account and the SuiteCloud Development Framework (SDF).
