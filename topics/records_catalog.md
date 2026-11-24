# Records Catalog
<!-- id: topic:records_catalog -->

See also: [SuiteQL](suiteql.md), [SuiteQL in N/query](query_definition.suiteql.md),
[Query definitions and components](query_definition.md), [SuiteScript Records Browser](records_browser.md),
[N/query](../modules/N_query.md).

The Records Catalog is a page inside a NetSuite account that describes that account's record types,
their fields and the joins between them. Because it reads the live account, it includes your
customizations and reflects the features you have enabled. Opening it requires the Records Catalog
permission, and what it shows respects the viewer's role permissions.

## Which reference to use for which API

Three references exist, and their IDs are not interchangeable:

| Reference | IDs valid for |
|---|---|
| [Records Browser](records_browser.md) (and this reference's [record pages](../gen/records/index.md)) | [N/search](../modules/N_search.md) and other modules, but not [N/query](../modules/N_query.md) |
| Records Catalog | [N/query](../modules/N_query.md) and SuiteQL |
| REST API Browser | REST web services, including REST SuiteQL |

The Records Catalog supplies the record and field names for:

- [N/query](../modules/N_query.md) query definitions;
- [SuiteQL](suiteql.md) statements;
- the REST query API;
- the workbook API ([N/workbook](../modules/N_workbook.md)).

## What it shows

Records are grouped into families (CRM, entity, item, transaction and other) and are marked as system
or custom.

For each field it shows the ID, name, column type, availability, the feature it depends on, the
permission it needs, and any join it supports. Some fields can be used only as joins.

For each join it shows the join type, the source and target records, the field ID, the cardinality and
availability.

## Channels and differences

The catalog's data model is shared by N/query and workbooks, and by SuiteAnalytics Connect through the
NetSuite2.com data source. Record types seen through Connect may differ slightly from what the catalog
shows, so check the Connect side separately if you rely on it.
