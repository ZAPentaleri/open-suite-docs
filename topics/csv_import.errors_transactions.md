# CSV import errors: transactions
<!-- id: topic:csv_import.errors_transactions -->

Part of: [CSV import](csv_import.md)

Errors seen when importing transactions, including journal entries, demand plans and payments
applied to bills or invoices. The first column describes the message; exact wording is not kept in
the source material behind this reference. An empty fix cell means the source gives only the cause.

> [!SOURCE-ERROR] The source's index of these messages lists the invalid department reference key
> error twice and leaves out the error about an invalid entity reference key for the currency. Both
> errors are in the tables below; do not rely on the source's index to find them.
<!-- source-error: topic:csv_import.errors_transactions | - | index_repeats_department_error -->

## Identifying the transaction

| Message (gist) | Cause | Fix |
|---|---|---|
| A unique identifier must be mapped | None of external ID, internal ID or the transaction number field is mapped. | |
| Record already exists | The external ID has been used before. | |
| Duplicate values in the key column | The same external ID appears more than once in the primary file. | |
| Invalid location reference key for the subsidiary | A demand plan is imported without an external ID. | Give it an external ID. |
| No blank headers allowed | The header row is missing. | |

## Lines and items

| Message (gist) | Cause | Fix |
|---|---|---|
| Line ambiguity error | The same item appears on several lines. | Map the line ID column, leaving it blank (or new) for lines to be added. |
| A line item is required | The item is supplied only as a default value. | Map a column to the item field. |
| A line item is required | The item field of the items sublist is not mapped. | Map that item field. |
| Invalid item reference key | The item name is formatted differently, or the item is inactive. | Refer to the item by internal ID. |
| Item to add is required | The line IDs are wrong. | |
| You must enter at least one line item | Expense lines were placed in the primary file. | Put the expenses in a separate file. |
| The record was deleted since you retrieved it | The line ID is mapped on an update that overwrites sublists. | Remove the line ID from the mapping. |
| Sales rep contributions exceed 100% | The entity's sales team was merged with the team given in the CSV. | Turn on *Overwrite sublists*. |

The invalid item reference key error appears twice in the source with the same cause and fix; it is
listed once here.

## Values and formats

| Message (gist) | Cause | Fix |
|---|---|---|
| Invalid transaction date value | The date is not in the user's preferred format. | |
| Invalid value for amount | The amount contains a thousands separator or a currency symbol. | |
| Rounding error | Debit or credit cells hold zeros or hidden decimal places. | Remove the zero amounts. |
| The transaction does not balance | A header-level amount is mapped. | Remove the header amount from the mapping. |
| Please enter a value for account | The file ends with a totals row. | Delete the totals row. |
| Amortization end date is before the start date | The start date is later than the end date. | |
| A type error from the year function (`getFullYear`) | A year has more than four digits. | |
| Use terms or a payment method, not both | Both fields are mapped. | |
| Please enter values for a field | The field is required on the form stored with the transaction. | Supply a value, or turn off the validation of mandatory fields. |

Updates to transactions are validated against the form stored on each transaction.

When a sales order is billed, its terms carry over to an invoice and its payment method to a cash
sale. The source does not explain how that choice is made beyond this.

## References to other records

| Message (gist) | Cause | Fix |
|---|---|---|
| Invalid account reference key for the subsidiary | The account is formatted differently, inactive, or belongs to another subsidiary. | |
| Invalid department reference key | The parent name is missing from the value, or the department is inactive. | |
| Invalid entity reference key for the currency | The entity is inactive, formatted differently, or does not use that currency. | |
| Invalid entity reference key | The vendor name is formatted differently, or the vendor is inactive. | Refer to the vendor by internal or external ID. |
| Invalid location reference key for the subsidiary | The location belongs to a different subsidiary. | |

## Payments applied to bills and invoices

When a customer payment's apply lines are matched, the internal ID is tried before the external ID.
Apply lines can be keyed only by internal ID or external ID.

| Message (gist) | Cause | Fix |
|---|---|---|
| Internal ID not found | The internal ID of the payment or of the invoice is wrong. | |
| No matching apply line | The same invoice ID appears on several lines. | Use each invoice ID on one line only. |
| No matching apply line | The bill or invoice is not open, or the ID is wrong. | |
| Invalid A/P account reference key | The A/P account is mapped together with the bill's internal ID. | Remove the A/P account from the mapping. |
| A payment was made since you retrieved the record | A terms discount was not deducted from the payment. | Pay the net amount. |
| Adding a line to apply is not allowed | The import runs in Add mode, the bill is closed, or the credit has already been applied. | Use Add or Update. |
| Unparseable internal ID | The bill value is not an internal ID. | |
