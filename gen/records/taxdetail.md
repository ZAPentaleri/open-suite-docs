# Tax Detail (`taxdetail`)
<!-- id: record:taxdetail -->

| Fact | Value |
|---|---|
| Internal ID | `taxdetail` |
| Tags | `search_join_only` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `account` | select | Account |  |
| `linenumber` | integer | Line Number |  |
| `taxamount` | currency | Tax Amount |  |
| `taxbasis` | currency | Tax Basis (Foreign Currency) |  |
| `taxcode` | select | Tax Code |  |
| `taxrate` | percent | Tax Rate |  |
| `taxtype` | select | Tax Type |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `account` | text | Account |  |
| `details` | textarea | Details |  |
| `linenumber` | integer | Line Number |  |
| `taxamount` | currency | Tax Amount |  |
| `taxbasis` | currency | Tax Basis (Foreign Currency) |  |
| `taxcode` | select | Tax Code |  |
| `taxfxamount` | currency | Tax Amount (Foreign Currency) |  |
| `taxrate` | percent | Tax Rate |  |
| `taxtype` | select | Tax Type |  |
