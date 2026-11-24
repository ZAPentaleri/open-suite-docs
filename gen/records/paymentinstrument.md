# Payment Instrument (`paymentinstrument`)
<!-- id: record:paymentinstrument -->

| Fact | Value |
|---|---|
| Internal ID | `paymentinstrument` |
| Tags | `deleted_record_search_supported`, `search_only` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `customer` | select | Customer |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Is Inactive |  |
| `paymentinstrumenttype` | select | Payment Instrument Type |  |
| `preserveonfile` | checkbox | Is Preserved On File |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `achaccounttype` | text | ACH: Account Type |  |
| `bankname` | text | ACH: Bank Name |  |
| `cardbrand` | text | Payment Card: Card Brand |  |
| `cardexpdate` | date | Payment Card: Expiration Date |  |
| `cardlastfourdigits` | text | Payment Card Token: Card Last Four Digits |  |
| `cardtokencardbrand` | text | Payment Card Token: Card Brand |  |
| `cardtokencardexpdate` | date | Payment Card Token: Card Expiration Date |  |
| `cardtokenexpdate` | date | Payment Card Token: Expiration Date |  |
| `cardtokennameoncard` | text | Payment Card Token: Name On Card |  |
| `customer` | select | Customer |  |
| `customerconsent` | text | Customer Consent |  |
| `default` | checkbox | Default |  |
| `externalid` | select | External ID |  |
| `generaltokenexpirationdate` | date | General Token: Expiration Date |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Is Inactive |  |
| `lastfourdigits` | text | Payment Card: Last Four Digits |  |
| `limit` | currency | Limit |  |
| `mask` | text | Mask |  |
| `nameoncard` | text | Payment Card: Name On Card |  |
| `ownername` | text | ACH: Owner Name |  |
| `paymentinstrumentstatetype` | select | Payment Card: Name On Card |  |
| `paymentinstrumenttype` | select | Payment Instrument Type |  |
| `paymentmethod` | select | Payment Method |  |
| `preserveonfile` | checkbox | Is Preserved on File |  |
| `routingnumber` | text | ACH: Routing Number |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `customer` | Customer | Customer |
| `transaction` | Transaction | Transaction |
