# Entity (`entity`)
<!-- id: record:entity -->

| Fact | Value |
|---|---|
| Internal ID | `entity` |
| Category | entity |
| Client scripts | no |
| Server scripts | no |
| Searchable | yes |
| Tags | `deleted_record_search_supported`, `search_only`, `supports_search` |

## Record facts

- `script_functions`: `search`

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `address` | textarea | Address |  |
| `addressee` | text | Addressee |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | phone | Address Phone |  |
| `attention` | text | Attention |  |
| `city` | text | City |  |
| `comments` | textarea | Comments |  |
| `country` | select | Country |  |
| `county` | text | County/State |  |
| `datecreated` | datetime | Date Created |  |
| `email` | text | Email |  |
| `entityid` | text | Name/ID |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `giveaccess` | checkbox | Login Access |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `image` | image | Image |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `level` | select | Level |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `state` | select | State/Province |  |
| `subsidiary` | select | Subsidiary |  |
| `type` | select | Entity Type |  |
| `zipcode` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `address` | text | Address |  |
| `addressee` | text | Addressee |  |
| `addressinternalid` | text | Address Internal ID |  |
| `addresslabel` | text | Address Label |  |
| `addressphone` | text | Address Phone |  |
| `altemail` | email | Alt. Email |  |
| `altname` | text | Name |  |
| `altphone` | phone | Office Phone |  |
| `attention` | text | Attention |  |
| `billcountrycode` | text | Billing Country Code |  |
| `billzipcode` | text | Billing Zip |  |
| `city` | text | City |  |
| `comments` | text | Comments |  |
| `country` | select | Country |  |
| `countrycode` | text | Country Code |  |
| `datecreated` | datetime | Date Created |  |
| `email` | email | Email |  |
| `entityid` | text | Name |  |
| `entitynumber` | integer | Number |  |
| `externalid` | select | External ID |  |
| `fax` | text | Fax |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `globalsubscriptionstatus` | select | Global Subscription Status |  |
| `image` | select | Image |  |
| `internalid` | select | Internal ID |  |
| `isdefaultbilling` | checkbox | Default Billing Address |  |
| `isdefaultshipping` | checkbox | Default Shipping Address |  |
| `isinactive` | checkbox | Inactive |  |
| `language` | select | Language |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `level` | select | Level |  |
| `permission` | select | Permission |  |
| `phone` | phone | Phone |  |
| `phoneticname` | text | Phonetic Name |  |
| `representingsubsidiary` | select | Represents Subsidiary |  |
| `shipcountrycode` | text | Shipping Country Code |  |
| `state` | select | State/Province |  |
| `statedisplayname` | select | State/Province Display Name |  |
| `subscription` | select | Subscription |  |
| `subscriptiondate` | datetime | Subscription Date |  |
| `subscriptionstatus` | checkbox | Subscription Status |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `type` | select | Type |  |
| `zipcode` | text | Zip Code |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `campaignresponse` | Campaign Response | Campaign |
| `messages` | Messages | Message |
| `messagesfrom` | Messages From | Message |
| `messagesto` | Messages To | Message |
| `user` | User | Employee |
