# Cardholder Authentication (`cardholderauthentication`)
<!-- id: record:cardholderauthentication -->

| Fact | Value |
|---|---|
| Internal ID | `cardholderauthentication` |

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `acceptheader` | text | Accept Header | no |  |  |  |
| `amount` | poscurrency | Amount | yes |  |  |  |
| `authenticatedeviceformaction` | text | Authenticate Device Form Action | no |  |  |  |
| `authenticatedeviceformid` | text | Form ID | no |  |  |  |
| `billingaddress` | summary | Bill To | no |  |  |  |
| `cavv` | text | Cardholder Authetication Verification Value (CAVV) | no |  |  |  |
| `challengeshopperformaction` | text | Challenge Shopper Form Action | no |  |  |  |
| `challengeshopperformid` | text | Form ID | no |  |  |  |
| `challengewindowsize` | select | Challenge window size | no |  |  |  |
| `currency` | select | Currency | yes |  |  |  |
| `eci` | select | E-Commerce Indicator (ECI) | no |  |  |  |
| `entity` | select | Cardholder | yes |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `handlingmode` | select | Handling Mode | yes |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `ipaddress` | text | IP Address | no |  |  |  |
| `notificationurl` | text | Notification | yes |  |  |  |
| `paymentoption` | select | Payment Option | yes |  |  |  |
| `paymentprocessingprofile` | select | Payment processing profile | yes |  |  |  |
| `shippingaddress` | summary | Ship To | no |  |  |  |
| `status` | select | Status | yes |  |  |  |
| `subsidiary` | select | Subsidiary | no |  |  |  |
| `threedstransactionid` | text | 3D Secure Transaction Id | no |  |  |  |
| `threedstranstatusreason` | select | 3D Secure Transaction Status Reason | no |  |  |  |
| `threedsversion` | select | 3D Secure Version | no |  |  |  |
| `useragent` | text | User Agent | no |  |  |  |

## Sublist `authenticatedeviceinputs` (Authenticate Device Input)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | no |  |  |
| `value` | clobtext | Value | no |  |  |

## Sublist `authenticatedeviceresults` (Authenticate Device Result)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | yes |  |  |
| `value` | clobtext | Value | yes |  |  |

## Sublist `authenticationfields` (Authentication Field)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | no |  |  |
| `value` | clobtext | Value | no |  |  |

## Sublist `cardholderauthenticationevents` (Cardholder Authentication Event)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `eventdatetime` | datetime | Date/Time | no |  |  |
| `isodate` | text | ISO Date | no |  |  |
| `paymentprocessingprofile` | select | Payment processing profile | no |  |  |
| `referencecode` | text | Reference Code | no |  |  |
| `request` | textarea | Raw Request | no |  |  |
| `response` | textarea | Raw Response | no |  |  |
| `status` | select | Status | no |  |  |
| `statusdetails` | text | Status Details | no |  |  |
| `statusreason` | select | Status Reason | no |  |  |
| `testmode` | checkbox | Test Mode | no |  |  |
| `threedstranstatus` | select | 3D Secure Transaction Status | no |  |  |
| `threedstranstatusreason` | select | 3D Secure Transaction Status Reason | no |  |  |
| `validationerror` | text | Plugin Validation Error | no |  |  |

## Sublist `challengeshopperinputs` (Challenge Shopper Input)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | no |  |  |
| `value` | clobtext | Value | no |  |  |

## Sublist `challengeshopperresults` (Challenge Shopper Result)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `name` | text | Name | yes |  |  |
| `value` | clobtext | Value | yes |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `authenticatedeviceformaction` | text | Authenticate Device Form Action |  |
| `authenticatedeviceformid` | text | Authenticate Device Form Id |  |
| `challengeshopperformaction` | text | Challenge Shopper Form Action |  |
| `challengeshopperformid` | text | Challenge Shopper Form Id |  |
| `datecreated` | datetime | Date Created |  |
| `entity` | select | Cardholder |  |
| `externalid` | select | External ID |  |
| `internalid` | select | Internal ID |  |
| `status` | select | Status |  |
