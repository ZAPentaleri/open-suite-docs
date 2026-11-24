# Preference Names and IDs
<!-- id: topic:suitescript_ids.preferences -->

Part of: [SuiteScript IDs](suitescript_ids.md)

See also: [N/config](../modules/N_config.md), [N/runtime](../modules/N_runtime.md),
[N/record](../modules/N_record.md).

Account and user preferences live on configuration pages. Each page has an ID, and every preference
on a page has a field ID. The full list of preference IDs with their labels is in
[catalog:preferences](../gen/catalogs/preferences.md).

## How scripts reach preferences

- [`config.load`](../modules/N_config.md) opens a configuration page as a record, picked through the
  [`config.Type`](../modules/N_config.md) enum; you then read or change its fields by preference ID.
- [`User.getPreference`](../modules/N_runtime.md) (on the object `runtime.getCurrentUser()` returns)
  reads a single preference value. It covers the General Preferences and Accounting Preferences pages.

## Finding a field ID in the UI

Turn on the Show Internal IDs option in Set Preferences. After that, clicking a field's label opens
the field help popup, which also shows the field's ID.

## Preferences listed under two labels

A few preference IDs appear twice in the source, under two labels. The catalog keeps one entry per
ID and records the second label:

| ID | Label | Also listed as |
|---|---|---|
| `SHOW_TRAN_NUMBERING_SETUP` | Show Transaction Numbering Setup | Asynchronous Job Plan Recalculation |
| `FORECASTWEIGHTED` | Calculate Forecasts as Weighted | Show Forecasts as Weighted |
| `JOURNALAPPROVALS` | Require Approvals On Journal Entries | Require Approvals on Journal Entries |
| `EXCHANGE_RATE_PROVIDER` | Rate Provider | Use Triangulation Calculation By NetSuite |

For `JOURNALAPPROVALS` the two labels differ only in capitalization. For the other three the labels
name different settings; see the notes under the relevant pages below.

## Configuration pages

Whether IDs on a page are case-sensitive differs from page to page; each section says which.

### Manufacturing preferences

- Page ID: `manufacturingpreferences`. IDs are not case-sensitive.
- In the UI: Setup > Manufacturing > Manufacturing Preferences.

> [!SOURCE-ERROR] The source heads this list as if it were the accounting periods page, though it
> contains the manufacturing preferences. Go by the page ID and UI location given here.
<!-- source-error: topic:suitescript_ids.preferences | - | page_called_accounting_periods -->

The ID given for "Average Cost Consumption Unit Cost" is `AVGCOSTSOMPLETIONSCOSTINGMETHOD` [sic].

> [!SOURCE-ERROR] This ID looks misspelt in the source (compare `AVGCOSTCOMPLETIONCOSTINGMETHOD` for
> "Average Cost Completion Unit Cost"). The spec keeps the source spelling; confirm the real field ID
> with Show Internal IDs before using it.
<!-- source-error: topic:suitescript_ids.preferences | - | consumption_unit_cost_id_spelled_avgcostsomplet -->

### Tax periods

- Page ID: `taxperiods`. IDs are not case-sensitive.
- In the UI: Setup > Accounting > Taxes > Manage Tax Periods.
- Example IDs: `fiscalmonth`, `fiscalyear`, `periodstyle`, `periodnameyear`.

### General preferences (company preferences)

- Page ID: `companypreferences`. IDs are not case-sensitive.
- In the UI: Setup > Company > General Preferences.

> [!SOURCE-ERROR] Three preference rows appear twice in the source's list for this page. Treat each
> of those preferences as a single entry.
<!-- source-error: topic:suitescript_ids.preferences | - | three_preference_rows_repeated -->

> [!SOURCE-ERROR] The row labelled "Asynchronous Job Plan Recalculation" shows the ID
> `SHOW_TRAN_NUMBERING_SETUP` [sic], which belongs to "Show Transaction Numbering Setup". The source
> gives no other ID for the recalculation preference; look it up with Show Internal IDs.
<!-- source-error: topic:suitescript_ids.preferences | - | async_job_plan_recalculation_shows_show_tran_numbering_setup -->

> [!SOURCE-ERROR] "Idle Session Timeout in Minutes" is listed as `IDLE_SESSION_TIMEOUT` [sic], but
> another source page (about the debugger) uses a different ID for the same setting. The spec does not
> say which is right; verify in your account.
<!-- source-error: topic:suitescript_ids.preferences | - | idle_session_timeout_id_differs_from_debugger_page -->

### Company information

- Page ID: `COMPANY_INFORMATION`. IDs on this page are treated as case-sensitive.
- In the UI: Setup > Company > Company Information.
- Example IDs: `companyname`, `legalname`, `formlogo`, `pagelogo`, `basecurrency`, `fiscalmonth`,
  `timezone`, `companyid`, `customersurl`.

> [!SOURCE-ERROR] The company information page states that preference IDs are case-sensitive, unlike
> most other configuration pages. The spec flags this for review without resolving it; match the case
> exactly to be safe.
<!-- source-error: topic:suitescript_ids.preferences | - | company_information_page_says_preference_ids_case_sensitive -->

In the UI each address is a text box with an Edit link that opens a popup holding more fields. The
text of each address box has its own ID:

- `mainaddress_text`
- `shippingaddress_text` [sic]
- `returnaddress_text`

> [!SOURCE-ERROR] The source prints these IDs with a space before the `_text` suffix. Write them
> without the space, as above.
<!-- source-error: topic:suitescript_ids.preferences | - | text_ids_shown_with_space_before_text_suffix -->

To read or change individual address fields, open the address as a subrecord with
[`Record.getSubrecord`](../modules/N_record.md); this is required for the address blocks. The
subrecord IDs are `mainaddress`, `shipaddress` and `returnaddress`. Fields on the address subrecord
include `addr1`, `addr2`, `attention`, `addressee`, `addrphone`, `city`, `state`, `zip` and `country`.

> [!SOURCE-ERROR] The shipping address text ID is spelled with `shippingaddress`, while the matching
> subrecord ID is `shipaddress`. The spec keeps both spellings as given; confirm which one your account
> accepts for each use.
<!-- source-error: topic:suitescript_ids.preferences | - | shippingaddress_text_vs_shipaddress_subrecord_id -->

### User preferences

- Page ID: `userpreferences`. IDs on this page are treated as case-sensitive.
- In the UI: Home > Set Preferences, with the subtabs General, Appearance, Transactions, Analytics,
  Activities, Alerts, Restrict View and Telephony.
- Whether a change applies to the current session only or permanently follows the same rules as in
  the UI.

> [!SOURCE-ERROR] Sources disagree about case: the user preferences page says preference IDs are
> case-sensitive, while most other configuration pages say their IDs are not. The spec keeps the user
> preferences page's statement; match the case exactly to be safe.
<!-- source-error: topic:suitescript_ids.preferences | - | user_preferences_page_says_preference_ids_case_sensitive -->

> [!SOURCE-ERROR] The "KPI Export Character Encoding" row shows `KPI_PERIOD_SPECIFIC_RATES` [sic],
> an ID whose name points to a different setting. Verify the encoding preference's ID in your account.
<!-- source-error: topic:suitescript_ids.preferences | - | kpi_export_encoding_row_shows_kpi_period_specific_rates_id -->

> [!SOURCE-ERROR] `FORECASTWEIGHTED` [sic] is given for two labels, "Calculate Forecasts as Weighted"
> and "Show Forecasts as Weighted". At most one pairing can be right; check which preference the ID
> controls before using it.
<!-- source-error: topic:suitescript_ids.preferences | - | forecastweighted_listed_for_two_labels -->

### Accounting preferences

- Page ID: `accountingpreferences`. IDs are not case-sensitive.
- In the UI: Setup > Accounting > Accounting Preferences, with the subtabs General,
  Items/Transactions, Order Management, Time & Expenses, and Approval Routing.

> [!SOURCE-ERROR] The rows for "Intercompany Expenses" and for journal approvals ("Require Approvals
> On Journal Entries") appear more than once in the source's list. Treat each as one preference.
<!-- source-error: topic:suitescript_ids.preferences | - | intercompany_expenses_and_journal_approvals_rows_repeated -->

> [!SOURCE-ERROR] `EXCHANGE_RATE_PROVIDER` [sic] is given as the ID of both "Rate Provider" and "Use
> Triangulation Calculation By NetSuite". The spec does not say which is right; verify in your account.
<!-- source-error: topic:suitescript_ids.preferences | - | exchange_rate_provider_id_given_for_two_labels -->

> [!SOURCE-ERROR] The label "Default Bill Quantity Variance Account" is paired with
> `BILLPRICEVARIANCEACCT` [sic], an ID that reads like the price variance account. Check the real ID
> before setting either account.
<!-- source-error: topic:suitescript_ids.preferences | - | bill_quantity_variance_label_shows_billpricevarianceacct -->

> [!SOURCE-ERROR] The ID for "Base Invoice Date on Billing Schedule Date" is spelled
> `INCOICEUSESCHEDULEDATE` [sic]. It may be the real ID or a typo; the spec keeps the source spelling,
> so verify it with Show Internal IDs.
<!-- source-error: topic:suitescript_ids.preferences | - | invoice_schedule_date_id_spelled_incoiceusescheduledate -->

### Accounting periods

- Page ID: `accountingperiods`. IDs are not case-sensitive.
- In the UI: Setup > Accounting > Manage G/L > Manage Accounting Periods.
- Example IDs: `fiscalmonth`, `fiscalyear`, `periodstyle`, `periodnameyear`, `lastday`.

### Tax preferences

- Page ID: `taxpreferences`. IDs are not case-sensitive.
- Each ID is the field ID followed by the nexus country code, for example `defaulttaxableus`,
  `chargeoutofdistrictus`, `perlinetaxesus`, `storeordertaxationus` and `enabletaxlookupus`.
- Which tax preference fields exist depends on the nexus.
- To look up the IDs, go to Setup > Accounting > Set Up Taxes and click the nexus.

> [!SOURCE-ERROR] The source names a `taxpreferences` page ID, but
> [`config.Type`](../modules/N_config.md) has no value for a tax preferences page, so the source does
> not show how to open this page with `config.load`. Verify in your account how (or whether) it can be
> loaded from a script.
<!-- source-error: topic:suitescript_ids.preferences | - | taxpreferences_page_id_has_no_config_type_value -->

## Example

A scheduled script that reads the city of the company's main address through the address subrecord,
and the running user's date format preference. The page and subrecord IDs come from the sections
above.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/config', 'N/runtime', 'N/log'], (config, runtime, log) => {
  const execute = () => {
    const companyInfo = config.load({ type: config.Type.COMPANY_INFORMATION });
    const mainAddr = companyInfo.getSubrecord({ fieldId: 'mainaddress' });
    const city = mainAddr.getValue({ fieldId: 'city' });
    const dateFormat = runtime.getCurrentUser().getPreference({ name: 'DATEFORMAT' });
    log.audit({ title: 'Head office', details: `City ${city}; dates shown as ${dateFormat}` });
  };

  return { execute };
});
```
