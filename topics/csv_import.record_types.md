# CSV import record types, sublists and subrecords
<!-- id: topic:csv_import.record_types -->

Part of: [CSV import](csv_import.md)

Which records, sublists and subrecords an import can reach, what each sublist needs before it can
be imported, and how sublist lines are matched on update.

## What is available

The record types offered in the Import Assistant depend on the user's role, the role's permissions
and the features enabled in the account. Sublists and subrecords follow the same logic: a feature
that is off takes them off the list.

- Importable sublists, with their per-sublist conditions:
  [csv_import_sublists](../gen/catalogs/csv_import_sublists.md)
- Importable subrecords: [csv_import_subrecords](../gen/catalogs/csv_import_subrecords.md)

Both catalogs are also referenced by [`N/task.CsvImportTask`](../modules/N_task.md#csvimporttask-object),
whose [`linkedFiles`](../modules/N_task.md#csvimporttasklinkedfiles) property supplies sublist and
subrecord data in separate files for a scripted multi-file import.

Single journal entries, budgets, single inventory worksheets, project tasks and web site redirects
use their own two-step assistants instead of the main one (and scripted imports cannot run them;
see [CSV import](csv_import.md)).

## Multi-line sublists

These sublists take several rows per record: apply, expense, inventory, item, line, member,
custom value, group member, price interval, price tiers, subline, tax details, order, location
regions and paycheck lines. How such rows are laid out in a single file is explained in
[single-file layout of sublist rows](csv_import.single_file_sublists.md).

Custom sublists cannot be imported.

## Subrecords

Subrecord data can arrive in one of two ways:

- in the record's own file, with extra rows for each nested level, or
- in a separate file per subrecord.

The two ways cannot be mixed: as soon as one subrecord sits in its own file, every subrecord must.
Splitting subrecords into files does not make the import any faster.

## Items

Item record types: assembly/bill of materials, description, discount, inventory, item group,
kit/package, markup, non-inventory, other charge, payment, service and subtotal. Several of them
also exist in lot-numbered and serialized variants, and in for-sale, for-resale and for-purchase
variants.

Item sublists: bills of materials, bin numbers, locations, members, merchandise hierarchy, pricing,
related items, site category, task templates, translation and vendors. Each may be supplied from
its own file. The sublist catalog has entries for all of these except bills of materials, so it
gives no conditions for that one.

> [!SOURCE-ERROR] The source's list of item sublists includes bills of materials, but its
> per-sublist table has no row for it. Treat the bills of materials sublist as importable only after
> checking it in the Import Assistant of your account; its features and keys are not stated.
<!-- source-error: topic:csv_import.record_types | - | sublist_table_omits_item_bills_of_materials -->

- If several vendors are marked preferred, only the last one is kept.
- Import bins before the items that use them.
- Matrix options must be set up in the UI before you import items that use them.
- Inventory history from before a OneWorld upgrade cannot be imported.

The item-specific conditions from the sublist catalog are under
[Item sublists in the catalog](#item-sublists-in-the-catalog) below.

## Updating sublists

- On a sublist that has key fields, an update can change selected lines (selective update) or
  replace the whole sublist.
- On a sublist without keys, an update either appends lines or replaces the whole sublist.
- No import can delete all of a sublist's data. A sublist that the UI never lets be empty is not
  protected by that rule during an import.
- A sublist's required fields are enforced only when that sublist is part of the import. In
  linked-file mode the import assumes the sublist is being imported.
- For custom transactions keyed on the account field, every line must use a different account.
- Journal entries need at least 2 lines and intercompany journal entries at least 4, and the lines
  must balance.

Which sublists have keys, and which fields those keys are, is listed per sublist in the catalog;
the next section summarizes it.

## Per-sublist conditions

Each of the 173 entries in [csv_import_sublists](../gen/catalogs/csv_import_sublists.md) names a
sublist within an import (its group, such as "Transactions: Vendor Credit"), and most carry facts
in a Facts column. 26 carry none (for example the paycheck sublists, the solution sublists, and the
credit memo apply and items sublists); for those the source states no special condition.

### Kinds of facts

| Fact | What it tells you |
|---|---|
| Required feature | The sublist is available only when that feature is enabled. Feature names are the exact NetSuite names. |
| Any one of several features | At least one of the listed features must be enabled; any one is enough. |
| Feature that must be disabled | The sublist is available only while that feature is off. |
| Required preference | A preference that must be enabled. |
| Other conditions | Extra prerequisites, such as more than one active accounting book, or at least one custom transaction type existing. |
| Selective-update key | The field or fields used to match a CSV row to an existing line, so an update can change that line. |
| Optional key | An extra key field that the source marks as optional. |
| Add-only or update-only | The sublist works only when adding new records, or only when updating existing ones. |
| Separate CSV file | The sublist's rows must come from a CSV file of their own. |
| Import types | The specific imports (for example particular item kinds) that do or do not offer the sublist. |
| Minimum lines | How many lines a record, or a new record, must have on the sublist. |
| Rules | Extra validity rules, such as debits and credits balancing. |

When an entry lists more than one key field, the catalog does not say whether all of them must be
mapped or any one is enough. Transaction item lines are the exception: they are matched by line
number or by item, as described under [Transaction line items](#transaction-line-items).

### Recurring patterns

- **Translations.** The translation sublists of accounts (chart of accounts), classes, departments,
  locations, custom lists and items need `Multi-Language` and are keyed on `Language`. The custom
  record translations sublist also needs `Multi-Language`; no key is given for it.
- **Accounting book detail.** On customer payments, expense reports, intercompany journal entries,
  inventory cost revaluations, invoices, journal entries, sales orders, vendor bills, vendor credits,
  vendor payments, vendor return authorizations and custom transactions, the
  `Accounting Book Detail` sublist needs `Multi-Book Accounting`, plus more than one active
  accounting book, and is keyed on `Accounting Book`. The `Accounting Books` sublist of checks,
  credit card charges and credit card refunds has the same feature and condition; on credit card
  charges and refunds it is keyed on `Line` and `Order Line`, and on checks no key is given. The
  `Accounting Books` sublists of custom transactions and transfer orders carry no facts.
- **Addresses.** The address sublists of employees, contacts, customers, jobs, leads, partners,
  prospects and vendors are keyed on `Internal ID` and `Label`.
- **Partners and sales teams.** Partner sublists on customers, leads, prospects, estimates,
  invoices, opportunities and sales orders need `Multi-Partner Management` and are keyed on
  `Internal ID`, `External ID` and `Partner`. Sales team sublists need `Team Selling`; no key is
  given for them, except on return authorizations (see below).
- **Subscriptions.** Contacts, customers, partners and vendors key their subscriptions sublist on
  `Subscription`; on employees the key is spelled `Subscriptions` [sic].

> [!SOURCE-ERROR] Only the employee subscriptions entry gives the key field in the plural; every other
> subscriptions sublist uses the singular. The source does not say which spelling the Import Assistant
> shows for employees, so check the field name in the mapping step.
<!-- source-error: topic:csv_import.record_types | - | subscriptions_key_field_plural_only_for_employees -->

- **Entity pricing and selling.** Customer, lead and prospect sublists: `Group Pricing` and
  `Item Pricing` need `Multiple Prices`, `Downloads` needs `Sell Files`, and the customer
  `Currencies` sublist needs `Multiple Currencies`. `Tax Registrations` on customers, partners and
  vendors need `SuiteTax`.
- **Sales transaction extras.** `Shipment` (cash sales, estimates, invoices, sales orders) needs
  `Multiple Shipping Routes`; `Auth. Code` (cash sales, invoices, sales orders) needs
  `Gift Certificates`.
- **Transaction items.** Cash sales, estimates, invoices, purchase orders, sales orders, vendor
  bills, vendor credits and vendor return authorizations key their `Items` sublist on
  `Line/Order Line` and `Item`, and a new record needs at least one line on it. Opportunities and
  transfer orders use the same keys without a stated minimum. Return authorizations key items on
  `Order Line` and `Item`; checks, credit card charges and credit card refunds key items on `Item`
  and expenses on `Line`.
- **Expenses that need their own file.** The `Expenses` sublists of purchase orders, vendor bills,
  vendor credits and vendor return authorizations, and the vendor credit `Apply` sublist, must each
  come from a separate CSV file. Purchase order expenses additionally need the preference
  `Allow Expenses on Purchase Orders`. Vendor credit and vendor return authorization expenses are
  keyed on `Line/Order Line`; vendor credit apply is keyed on `Apply` and `Line`.
- **Payments.** Customer payment `Invoices` are keyed on `Invoice` and vendor payment `Bills` on
  `Bill`; in both, `Line` is an optional extra key.
- **Journal entries.** The journal entry line sublist needs at least 2 lines whose debits and
  credits balance. Intercompany and advanced intercompany journal entry lines need at least 4
  lines, with a debit and a credit for both the originating and the receiving subsidiary, and they
  must balance too. The statistical journal entry `Line` sublist needs at least one line on a new
  record. No facts are given for the period end journal entry line sublist.
- **Payroll.** The employee payroll sublists (`Payroll Accrued Times`, `Payroll Contributions`,
  `Payroll Deductions`, `Payroll Earnings`) need `Payroll` and are keyed on `Payroll Item`.
  `Direct Deposit` needs the `Direct Deposit` feature and is keyed on `Internal ID`.
- **SuiteBilling.** Price book lines (keyed on `Subscription Plan Line`), price plan pricing (keyed
  on `Above Quantity`), subscription plan members, subscription price book lines and subscription
  change order items (keyed on `Line Number`) all need `SuiteBilling`.
- **Activities.** Event attendees are keyed on `Attendee` and event resources on `Resource`. Phone
  call contacts are keyed on `Participant` and `Contact`; task contacts on `Company/Project` and
  `Contact`. Time tracking on phone calls and tasks needs `Time Tracking`.

### Add-only and update-only sublists

- `Credit Cards` on customers, leads and prospects can be imported when adding new records, but
  updating existing credit card lines is not supported.
- `Contact Access` on customers and partners is available only when updating existing records. It
  is meant for adding or changing the access of contacts that already exist, and is keyed on
  `Contact`.

### Item sublists in the catalog

| Sublist | Needs | Key | Offered in |
|---|---|---|---|
| Bin Numbers | any one of `Bin Management`, `Advanced Bin/Numbered Inventory Management` | `Bin Number` | `Assembly/BOM Item`, `Inventory Item` imports; import bins before items with bin numbers |
| Locations | `Multi-Location Inventory` | `Internal ID`, `External ID`, `Location` | `Assembly/BOM Item`, `Inventory Item` |
| Members | | | `Assembly/BOM Item`, `Item Group`, `Kit/Package Item` |
| Merchandise Hierarchy | `Merchandise Hierarchy` | | `Inventory Item`, `Lot Numbered Inventory Item`, `Serialized Inventory Item` |
| Pricing | any one of `Multiple Prices`, `Quantity Pricing`, `Multiple Currencies` | `Currency`, `Quantity`, `Level`, depending on the enabled features | |
| Related Items | `Web Site` | | not in `Non-Inventory Item for Purchase`, `Service Item for Purchase`, `Other Charge Item` |
| Site Category | `Web Site` | | not in `Non-Inventory Item for Purchase`, `Service Item for Purchase`, `Other Charge Item` |
| Task Templates | `Project Management` | | `Service Item for Sale` only |
| Translations | `Multi-Language` | `Language` | |
| Vendors | `Multiple Vendors` | `Vendor` | `Inventory Item`, `Non-Inventory Item for Resale or Purchase`, `Service Item for Resale or Purchase`, `Other Charge Item for Resale or Purchase`; also `Assembly/BOM Item` when purchasing assembly items is allowed |

An empty cell means the catalog states nothing for that column. There is no row for bills of
materials, although the item sublist list above names it.

> [!SOURCE-ERROR] A second statement in the source likewise leaves bills of materials out of the
> per-sublist table while listing it as an item sublist. Its required features, keys and import types
> are therefore unknown; confirm them in your account before relying on such an import.
<!-- source-error: topic:csv_import.record_types | - | sublist_table_omits_item_bills_of_materials -->

### Notable single cases

- **Job addresses** are available only while `Project Management` is **disabled**; the job
  `Resource` sublist, by contrast, needs `Project Management` enabled.
- **Contact addresses** are offered only in the `Contacts Only` import, not in
  `Customers and Contacts Together`, `Leads and Contacts Together` or
  `Prospects and Contacts Together`.
- **Expense category rates** are available only in `OneWorld` and are keyed on `Subsidiary`.
- **Custom transaction lines** need `Custom Transactions` and at least one custom transaction type.
  They are keyed on `Line` and `Account`; `Account` alone works as the key only when every line
  has a different account.
- **Expense report expenses** need `Expense Reports` and are keyed on `Line Id` and `Ref No.`.
- **Custom list values** are keyed on `ID`; **group members** on `Internal ID`, `External ID` and
  `Names`.
- **Return authorizations** key partners on `Partner` only and the sales team on `Employee`.
- **Inventory cost revaluation** cost components need `Standard Costing` and are keyed on
  `Component` and `Cost Category`.
- **Manufacturing**: routing steps are keyed on `Operation Sequence`, and both routing steps and
  manufacturing cost template cost detail need at least one line on a new record. Bill of materials
  revision components need `Advanced Bill of Materials`.
- **Inventory transfer** `Adjustments - Inventory Detail` needs
  `Advanced Bin/Numbered inventory Management` [sic], written there with a lower-case "inventory".
  The item bin numbers entry capitalizes the same name.

> [!SOURCE-ERROR] The same feature name is capitalized differently in two places of the source
> (lower-case "inventory" on the inventory transfer entry). Treat both as one feature, and use the
> spelling your account's Enable Features page shows.
<!-- source-error: topic:csv_import.record_types | - | feature_name_case_differs_advanced_bin_numbered_inventory -->

- **Foreign currency variance posting rule** criteria need `Foreign Currency Variance Mapping`.
- **Item demand plan** demand planning lines: the fields vary with the demand plan type, which can
  be monthly, weekly or daily.
- **Merchandise hierarchy node** `Hierarchy Versions`: its UI field `Included in Version` marks the
  node as assigned to a hierarchy version, and a node can belong to several versions.
- **Billable time** on cash sales and invoices is keyed on `related transaction ID`. The source's
  notes for these two entries contain a leftover placeholder; see the flags on the
  [catalog page](../gen/catalogs/csv_import_sublists.md#suspected-source-errors).
- **Cash sale partners** are listed with `Team Selling` [sic] as their feature, while every other
  partner sublist needs `Multi-Partner Management`; the catalog page flags this, and the source
  does not say which is right. Keys: `Internal ID`, `External ID`, `Partner`.

## Transaction line items

The rules below apply to updating line items on cash sales, estimates, invoices, opportunities,
purchase orders, sales orders, vendor bills, vendor credits and vendor return authorizations.

**When items are required.** Adding a cash sale, estimate, invoice, purchase order, sales order,
vendor bill, vendor credit or vendor return authorization requires item lines (the catalog puts the
minimum at one line for new records). Update and Add-or-Update imports do not require them, and
neither do opportunities.

**How lines are matched.** An existing line is identified by its line number (the `Line/Order Line`
key) or by its item. If both are mapped, the line number wins.

- A row whose key matches an existing line updates that line; any other row becomes a new line.
- A blank line-number value adds a new line.
- If the item is the key and the same item appears on more than one line, the match is ambiguous
  and the row fails with an error.
- Always map the line column, even when some of its cells are blank.
- Line fields you do not map stay as they are.
- To remove a line, import the transaction again with *Overwrite sublists* on and leave that line
  out (see [advanced options](csv_import.advanced_options.md)).
