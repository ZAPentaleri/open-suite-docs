# Custom field sourcing
<!-- id: topic:custom_fields.sourcing -->

Part of: [Custom fields](custom_fields.md)

Sourcing copies a value from a related record into a custom field. You pick a **source** (a List/Record
field on the record, such as the entity on a transaction) and a **source field** on the record it points
to. The general rule: source a standard field only into a custom field of the matching type (for
example, a phone number into a phone number field).

The tables below list, per source, the standard fields you can copy and the custom field type each one
needs.

## Transaction body fields

A transaction body field can be sourced from the **entity**, the **ship-to** entity or the **sales rep**.
The entity is a customer on sales transactions, a vendor on purchase transactions and an employee on
expense reports, so the available fields depend on the transaction type.

### Entity fields

| Source field | Custom field type | Available on |
|---|---|---|
| Name | Free-form text | All transactions |
| Bill To | Text area | All transactions |
| Phone | Phone number | All transactions |
| Email | Email address | All transactions |
| City | Free-form text | All transactions |
| Country | Free-form text | All transactions |
| State | Free-form text | All transactions |
| Zip | Free-form text | All transactions |
| Ship To | Text area | Sales and purchase |
| Fax | Phone number | Sales and purchase |
| Contact | Free-form text | Sales and purchase |
| Alt. Contact | Free-form text | Sales and purchase |
| Alt. Phone | Phone number | Sales and purchase |
| Balance | Currency | Sales and purchase |
| Credit Limit | Currency | Sales and purchase |
| Account | Free-form text | Sales and purchase |
| Sales Rep | List/Record | Sales only |
| Expected Close Date | Date | Sales only |
| Renewal Date | Date | Sales only |
| 1099 Eligible | Check box | Purchase only |
| Tax ID | Free-form text | Purchase only |
| Legal Name | Free-form text | Purchase only |
| Supervisor | List/Record | Expense reports only |
| Soc. Sec. Number | Free-form text | Expense reports only |

### Ship-to fields

Name (free-form text), Bill To (text area), Phone (phone number), Email (email address), City
(free-form text), Country (free-form text), Ship To (text area), Fax (phone number), State (free-form
text), Sales Rep (List/Record), Expected Close Date (date), Renewal Date (date), Contact (free-form
text), Alt. Contact (free-form text), Alt. Phone (phone number), Balance (currency) and Credit Limit
(currency).

> [!SOURCE-ERROR] For the ship-to source (whose fields are customer fields), the source's note on the
> List/Record field type points to the Employee list, which does not fit a customer-based source. Check
> which list the sourced List/Record field actually uses in your account.
<!-- source-error: topic:custom_fields.sourcing | - | list_record_note_says_employee_list_for_customer_sources -->

### Sales rep fields

Name (free-form text), Bill To (text area), Phone (phone number), Email (email address), City
(free-form text), Country (free-form text), Supervisor (List/Record) and Soc. Sec. Number (free-form
text).

## Transaction line fields

A transaction line field can be sourced from the line's **item** or its **customer**.

**Item fields** (also the only source for transaction item option fields): Name (free-form text),
Display Name (free-form text), Vendor Name (free-form text), Online Name (free-form text), Available
Online (check box), Base Price (currency), Cost (currency), Preferred Vendor (List/Record) and On Hand
(decimal number).

**Customer fields**: Name (free-form text), Bill To (text area), Phone (phone number), Email (email
address), City (free-form text), Country (free-form text), Ship To (text area), Fax (phone number),
State (free-form text), Sales Rep (List/Record), Expected Close Date (date), Renewal Date (date),
Contact (free-form text), Alt. Contact (free-form text), Alt. Phone (phone number), Balance (currency),
Credit Limit (currency) and Account (free-form text).

## Transaction item option fields

The only source is the **item**, with the same item fields listed above.

## CRM fields

CRM fields (the case record fields) can be sourced from the **company**, the **contact**, the **item**
or the **assigned** person.

- **Company or contact**: the same eighteen fields as the line-level customer source above, from Name
  through Account.
- **Item**: the item fields listed above.
- **Assigned**: Name (free-form text), Bill To (text area), Phone (phone number), Email (email
  address), City (free-form text), Country (free-form text), Supervisor (List/Record) and Soc. Sec.
  Number (free-form text).

## Entity fields

Entity custom fields can be sourced from the **sales rep** or the **supervisor**. Either one offers:
Name (free-form text), Bill To (text area), Phone (phone number), Email (email address), City
(free-form text), Country (free-form text), Supervisor (List/Record), Soc. Sec. Number (free-form
text), State (free-form text) and Zip (free-form text).

## Item fields

Item custom fields can be sourced from the **preferred vendor**, which offers: Name (free-form text),
Bill To (text area), Ship To (text area), Phone (phone number), Fax (phone number), Email (email
address), City, State, Zip and Country (free-form text each), Contact and Alt. Contact (free-form
text), Alt. Phone (phone number), Balance and Credit Limit (currency), Account (free-form text),
1099 Eligible (check box), Tax ID (free-form text) and Legal Name (free-form text).
