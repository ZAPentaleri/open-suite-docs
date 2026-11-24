# Payment Event (`paymentevent`)
<!-- id: record:paymentevent -->

| Fact | Value |
|---|---|
| Internal ID | `paymentevent` |
| Tags | `deleted_record_search_supported`, `search_only` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `adjustedexpirationdateused` | checkbox | Processed with Adjusted Expiration Date |  |
| `amount` | currency | Amount |  |
| `authcode` | text | Auth. Code |  |
| `avsstreetmatch` | select | AVS Street Match |  |
| `avszipmatch` | select | AVS Zip Match |  |
| `cancelable` | checkbox | Cancelable |  |
| `cccustomercode` | text | Card Customer Code |  |
| `ccexpdate` | date | Card Expires |  |
| `ccholdername` | text | Cardholder Name |  |
| `ccnumber` | checkbox | Payment Card Number |  |
| `cscmatch` | select | CSC Match |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `date` | datetime | Payment Event Date |  |
| `holdreason` | select | Payment Status Reason |  |
| `initialusereference` | text | Initial Use Reference |  |
| `instrumentpreservedonfile` | checkbox | Instrument Preserved On File |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinitialuse` | checkbox | Is Initial Use |  |
| `isrecurringpayment` | checkbox | Recurring Payment |  |
| `lastpaymentevent` | checkbox | Last Payment Event |  |
| `maskedcard` | text | Masked Card |  |
| `merchantaccount` | select | Payment Processing Profile |  |
| `merchantprintblock` | text | Merchant Print Block |  |
| `mitpurpose` | select | Mit Purpose |  |
| `newinitialusereference` | text | New Initial Use Reference |  |
| `paymentapproved` | checkbox | Payment Approved |  |
| `paymenteventtype` | select | Payment Operation |  |
| `paymentinitiator` | select | Payment Initiator |  |
| `paymentmethod` | select | Payment Method |  |
| `paymentrequestid` | text | Payment Request Id |  |
| `paymentstatus` | select | Payment Event Result |  |
| `pnrefnum` | text | P/N Ref |  |
| `processedexternally` | checkbox | Processed Externally |  |
| `purchasecardused` | checkbox | Purchase Card Used |  |
| `purchasedatasent` | checkbox | Purchase Data Sent |  |
| `remainingamount` | currency | Remaining Amount |  |
| `shopperprintblock` | text | Shopper Print Block |  |
| `signaturerequired` | checkbox | Signature Required |  |
| `testmode` | checkbox | Test Mode |  |
| `transaction` | select | Transaction |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `adjustedexpirationdateused` | checkbox | Processed with Adjusted Expiration Date |  |
| `amount` | currency | Amount |  |
| `authcode` | text | Auth. Code |  |
| `avsstreetmatch` | select | AVS Street Match |  |
| `avszipmatch` | select | AVS Zip Match |  |
| `cancelable` | checkbox | Cancelable |  |
| `cccustomercode` | text | Card Customer Code |  |
| `ccexpdate` | date | Card Expires |  |
| `ccholdername` | text | Cardholder Name |  |
| `ccnumber` | text | Payment Card Number |  |
| `ccstreet` | text | Card Street |  |
| `cczipcode` | text | Card Zip Code |  |
| `cscmatch` | select | CSC Match |  |
| `currency` | select | Currency |  |
| `customer` | select | Customer |  |
| `date` | datetime | Payment Event Date |  |
| `holddetails` | text | Payment Status Reason Details |  |
| `holdreason` | select | Payment Status Reason |  |
| `initialusereference` | text | Initial Use Reference |  |
| `instrumentpreservedonfile` | checkbox | Instrument Preserved On File |  |
| `internalid` | select | Internal ID |  |
| `isinitialuse` | checkbox | Is Initial Use |  |
| `isodate` | text | ISO Date |  |
| `isrecurringpayment` | checkbox | Recurring Payment |  |
| `lastpaymentevent` | checkbox | Last Payment Event |  |
| `maskedcard` | text | Masked Card |  |
| `merchantaccount` | select | Payment Processing Profile |  |
| `merchantprintblock` | text | Merchant Print Block |  |
| `mitpurpose` | select | Mit Purpose |  |
| `newinitialusereference` | text | New Initial Use Reference |  |
| `paymentapproved` | checkbox | Payment Approved |  |
| `paymenteventtype` | select | Payment Operation |  |
| `paymentinitiator` | select | Payment Initiator |  |
| `paymentmethod` | select | Payment Method |  |
| `paymentrequestid` | text | Payment Request Id |  |
| `paymentstatus` | select | Payment Event Result |  |
| `pnrefnum` | text | P/N Ref |  |
| `processedexternally` | checkbox | Processed Externally |  |
| `purchasecardused` | checkbox | Purchase Card Used |  |
| `purchasedatasent` | checkbox | Purchase Data Sent |  |
| `remainingamount` | currency | Remaining Amount |  |
| `shopperprintblock` | text | Shopper Print Block |  |
| `signaturerequired` | checkbox | Signature Required |  |
| `testmode` | checkbox | Test Mode |  |
| `transaction` | select | Transaction |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `transaction` | Transaction | Transaction |
