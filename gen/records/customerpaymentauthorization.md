# Customer Payment Authorization (`customerpaymentauthorization`)
<!-- id: record:customerpaymentauthorization -->

| Fact | Value |
|---|---|
| Internal ID | `customerpaymentauthorization` |
| Search data via | Transaction |
| Tags | `custom_fields_supported` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `allowemptycards` | checkbox | Credits | no | no |  |  |
| `authcode` | text | Auth. Code | no | no |  |  |
| `ccapproved` | checkbox | CC Approved | no | no |  |  |
| `ccavsstreetmatch` | select | AVS Street Match | no | no |  |  |
| `ccavszipmatch` | select | AVS Zip Match | no | no |  |  |
| `ccexpiredate` | ccexpdate | Expires (MM/YYYY) | no | no |  |  |
| `cchold` | text | Detail | no | no |  |  |
| `ccholdetails` | textarea | Detail | no | no |  |  |
| `cciavsmatch` | select | IAVS Match | no | no |  |  |
| `ccname` | text | Name on Card | no | no |  |  |
| `ccnumber` | ccnumber | Credit Card # | no | no |  |  |
| `ccprocessoraccount` | text | Payment Processing Profile | no | no |  |  |
| `ccsecuritycode` | text | CSC | no | no |  |  |
| `ccsecuritycodematch` | select | CSC Match | no | no |  |  |
| `ccstreet` | text | Card Street | no | no |  |  |
| `cczipcode` | text | Card Zip Code | no | no |  |  |
| `class` | select | Class | no | no |  |  |
| `createddate` | datetime |  | no | no |  |  |
| `creditcard` | select | Credit Card Select | no | no |  |  |
| `creditcardprocessor` | select | Payment Processing Profile | no | no |  |  |
| `currency` | select | Currency | no | no |  |  |
| `currencyname` | text | Currency | no | no |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |
| `customer` | select | Customer | no | no |  |  |
| `customercode` | text | Customer Code | no | no |  |  |
| `customform` | select | Custom Form | yes | no |  |  |
| `debitcardissueno` | text | Issue No. | no | no |  |  |
| `department` | select | Department | no | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `getauth` | checkbox | Get Authorization | no | no |  |  |
| `ignoreavs` | checkbox | Ignore AVS | no | no |  |  |
| `ignorecsc` | checkbox | Ignore CSC | no | no |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |
| `isrecurringpayment` | checkbox | Recurring Payment | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `location` | select | Location | no | no |  |  |
| `memo` | text | Memo | no | yes |  |  |
| `overridehold` | checkbox | Override Hold | no | no |  |  |
| `overrideholdchecked` | checkbox | Override Hold | no | no |  |  |
| `payment` | poscurrency | Payment Amount | yes | no |  |  |
| `paymenteventholdreason` | select | Reason | no | no |  |  |
| `paymenteventresult` | select | Status | no | no |  |  |
| `paymentmethod` | select | Payment Method | no | no |  |  |
| `pnrefnum` | text | P/N Ref. | no | no |  |  |
| `redirecturl` | url | Redirect to URL | no | no |  |  |
| `returnurl` | url | Redirect From URL | no | no |  |  |
| `salesorder` | select | Sales Order | no | no |  |  |
| `subsidiary` | select | Subsidiary | no | no |  |  |
| `threedstatuscode` | text | Payer Authentication Status | no | no |  |  |
| `trandate` | date | Date | yes | no |  |  |
| `tranid` | text | Authorization # | no | no |  |  |
| `transactionnumber` | text | Transaction Number | no | no |  |  |
| `updatecurrency` | text | Update Currency | no | no |  |  |
| `validfrom` | ccvalidfrom | Valid From / Start Date (MM/YYYY) | no | no |  |  |
