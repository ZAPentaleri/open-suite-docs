# N/format/i18n
<!-- id: N/format/i18n -->

`N/format/i18n` produces strings that suit an international audience. It covers three jobs:
locale-aware number and currency strings, formatting and parsing phone numbers, and spelling a
number out in words in a chosen language.

- **Availability:** client and server scripts.
- **Load it as:** `define(['N/format/i18n'], (i18n) => { ... })`
- Related: [N/format](N_format.md) (formatting by the user's own preferences).

**How the module is organised.** The factory methods (`getCurrencyFormatter`, `getNumberFormatter`,
`getPhoneNumberFormatter`, `getPhoneNumberParser`, `spellOut`) and the enums (`Currency`,
`NegativeNumberFormat`, `PhoneNumberFormatType`, `Country`) are members of an object named `format`.
That `format` object is the module object itself: it is what `define` or `require` binds when you
load `N/format/i18n`. With the conventional binding name `i18n`, you therefore call
`i18n.getNumberFormatter(...)` and read `i18n.Country.IRELAND`. Parameter types in this page are
written as the source gives them (`format.Country`, `format.PhoneNumberFormatType`, ...), where
`format` means this same module object.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`format`](#format-object) | object | The module object: factory methods and enums |
| [`format.getCurrencyFormatter(options)`](#formatgetcurrencyformatteroptions) | method | Build a formatter for currency amounts |
| [`format.getNumberFormatter(options)`](#formatgetnumberformatteroptions) | method | Build a formatter for plain numbers |
| [`format.getPhoneNumberFormatter(options)`](#formatgetphonenumberformatteroptions) | method | Build a formatter for phone number objects |
| [`format.getPhoneNumberParser(options)`](#formatgetphonenumberparseroptions) | method | Build a parser for phone number strings |
| [`format.spellOut(options)`](#formatspelloutoptions) | method | Write a number out in words |
| [`CurrencyFormatter`](#currencyformatter-object) | object | Number to currency string |
| [`NumberFormatter`](#numberformatter-object) | object | Number to locale string |
| [`PhoneNumberFormatter`](#phonenumberformatter-object) | object | Phone number object to string |
| [`PhoneNumberParser`](#phonenumberparser-object) | object | Phone number string to object |
| [`format.Currency`](#formatcurrency) | enum | Currency codes |
| [`format.NegativeNumberFormat`](#formatnegativenumberformat) | enum | How negative numbers are shown |
| [`format.PhoneNumberFormatType`](#formatphonenumberformattype) | enum | Phone number output styles |
| [`format.Country`](#formatcountry) | enum | Countries for phone parsing |

## Objects

### format (object)
<!-- id: N/format/i18n.format -->

The object that holds this module's factory methods and enums. It is the value bound by `require`
or `define` for `N/format/i18n`, so in code you reach its members directly on your module binding
(for example `i18n.spellOut(...)`). It has no properties of its own.

#### format.getCurrencyFormatter(options)
<!-- id: N/format/i18n.format#getCurrencyFormatter -->

Creates a [`CurrencyFormatter`](#currencyformatter-object) that renders numbers as currency
strings. You identify the formatting either by a currency code or by a locale, and you must give
exactly one of the two: leaving both out is an error, and so is passing both.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.currency` | string | No | |
| `options.locale` | string | No | |

- `options.currency`: conditionally required; you need it whenever `options.locale` is absent. Takes
  a value from [`format.Currency`](#formatcurrency). Since 2019.2.
- `options.locale`: conditionally required; you need it whenever `options.currency` is absent.
  Since 2021.1.
- The two options are mutually exclusive: never pass both in one call.

**Returns:** `Object`, a [`CurrencyFormatter`](#currencyformatter-object).

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.currency` was needed but not supplied.
- `SSS_INVALID_CURRENCY`: `options.currency` is not a valid value.
- `SSS_INVALID_TYPE_ARG`: an argument has the wrong type.
- `NEITHER_ARGUMENT_DEFINED`: neither `options.currency` nor `options.locale` was given.
- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: both `options.currency` and `options.locale` were given.
- `INVALID_LOCALE`: `options.locale` is not a valid locale.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### format.getNumberFormatter(options)
<!-- id: N/format/i18n.format#getNumberFormatter -->

Creates a [`NumberFormatter`](#numberformatter-object) for plain numbers. A locale supplies the
starting settings; any of the other options you pass (separators, precision, negative style) take
precedence over what the locale would choose.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.negativeNumberFormat` | format.NegativeNumberFormat | No | |
| `options.locale` | string | No | |
| `options.groupSeparator` | string | No | |
| `options.decimalSeparator` | string | No | |
| `options.precision` | number | No | |

- `options.negativeNumberFormat`: a value from
  [`format.NegativeNumberFormat`](#formatnegativenumberformat). Since 2019.2.
- `options.locale`: the source of the default settings; the other formatter options override it.
  Since 2021.1.
- `options.groupSeparator`, `options.decimalSeparator`, `options.precision`: since 2019.2. The source
  states no defaults for them.

**Returns:** `Object`, a [`NumberFormatter`](#numberformatter-object).

**Errors:**

- `INVALID_LOCALE`: the locale value is not valid.
- `SSS_INVALID_TYPE_ARG`: an argument value is not valid.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### format.getPhoneNumberFormatter(options)
<!-- id: N/format/i18n.format#getPhoneNumberFormatter -->

Creates a [`PhoneNumberFormatter`](#phonenumberformatter-object) that writes phone number objects
in one particular style.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.formatType` | format.PhoneNumberFormatType | Yes | |

- `options.formatType`: a value from [`format.PhoneNumberFormatType`](#formatphonenumberformattype).
  Required; this option dates from 2020.2, while the method itself dates from 2019.2.

> [!SOURCE-ERROR] The source marks `options.formatType` as required since 2020.2, yet its own sample
> calls this method without any options. Pass `formatType` explicitly; do not rely on the sample's
> no-argument form.
<!-- source-error: N/format/i18n.format#getPhoneNumberFormatter | - | formattype_required_since_2020_2_sample_omits_options -->

**Returns:** `Object`, a [`PhoneNumberFormatter`](#phonenumberformatter-object).

**Errors:**

- `SSS_INVALID_FORMAT_TYPE`: `options.formatType` is not an accepted value.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### format.getPhoneNumberParser(options)
<!-- id: N/format/i18n.format#getPhoneNumberParser -->

Creates a [`PhoneNumberParser`](#phonenumberparser-object). The parser needs to know which country
to assume when a number arrives without an international prefix; that is what `defaultCountry`
supplies.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.defaultCountry` | format.Country | No | |

- `options.defaultCountry`: a value from [`format.Country`](#formatcountry). If you leave it out,
  the company's country is used. You need it when the numbers you parse are not written in
  international format. Since 2020.2.

**Returns:** `Object`, a [`PhoneNumberParser`](#phonenumberparser-object).

**Errors:**

- `SSS_INVALID_COUNTRY_ID`: `options.defaultCountry` is not an accepted value.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### format.spellOut(options)
<!-- id: N/format/i18n.format#spellOut -->

Returns a number written out as words in the language you name. Both positive and negative numbers
are accepted.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.locale` | string | Yes | |
| `options.number` | number | Yes | |

- `options.locale`: an ISO 639-1 language code (two letters, such as `de`). The language used is the
  one you pass, regardless of the account's language. Since 2019.1.
- `options.number`: the number to spell out. Since 2019.1.

**Returns:** `String`, the number in words in the requested language.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.1

### CurrencyFormatter (object)
<!-- id: N/format/i18n.CurrencyFormatter -->

Converts numbers into currency strings. You get one from
[`format.getCurrencyFormatter(options)`](#formatgetcurrencyformatteroptions). Client and server
scripts, since 2019.2.

#### CurrencyFormatter.format(options)
<!-- id: N/format/i18n.CurrencyFormatter#format -->

Formats a number as a currency string using this formatter's settings.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.number` | number | Yes | |

**Returns:** `String`, the amount as a currency string.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### CurrencyFormatter.currency
<!-- id: N/format/i18n.CurrencyFormatter#currency -->

`string`, read-only. The currency code the formatter uses. Since 2019.2.

#### CurrencyFormatter.symbol
<!-- id: N/format/i18n.CurrencyFormatter#symbol -->

`string`, read-only. The currency symbol the formatter uses. Since 2019.2.

#### CurrencyFormatter.numberFormatter
<!-- id: N/format/i18n.CurrencyFormatter#numberFormatter -->

Read-only. A [`NumberFormatter`](#numberformatter-object) that formats numbers without the currency
symbol, useful when you want the digits laid out the same way but no symbol. Its recorded type is
`string`. Since 2019.2.

> [!SOURCE-ERROR] The source's type row calls this property a `string`, yet it describes the value
> as a number formatter. Treat it as a `NumberFormatter` object and check the actual value at runtime.
<!-- source-error: N/format/i18n.CurrencyFormatter#numberFormatter | - | type_row_says_string_for_numberformatter -->

#### CurrencyFormatter.locale
<!-- id: N/format/i18n.CurrencyFormatter#locale -->

`string`, read-only. The locale the formatter uses. Since 2021.1.

### NumberFormatter (object)
<!-- id: N/format/i18n.NumberFormatter -->

Converts numbers into strings. You get one from
[`format.getNumberFormatter(options)`](#formatgetnumberformatteroptions) (or through
[`CurrencyFormatter.numberFormatter`](#currencyformatternumberformatter)). Client and server
scripts, since 2019.2.

#### NumberFormatter.format(options)
<!-- id: N/format/i18n.NumberFormatter#format -->

Formats a number as a string using this formatter's settings.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.number` | number | Yes | |

**Returns:** `String`, the formatted number.

**Governance:** 10 units.

**Availability:** client and server scripts.

**Since:** 2019.2

#### NumberFormatter.groupSeparator
<!-- id: N/format/i18n.NumberFormatter#groupSeparator -->

`string`, read-only. The group separator the formatter uses. Since 2019.2.

> [!SOURCE-ERROR] The source's table for this property names `CurrencyFormatter` as the owning
> object, although the property is documented as part of `NumberFormatter`. Verify against a real
> formatter instance.
<!-- source-error: N/format/i18n.NumberFormatter#groupSeparator | - | parent_object_row_says_currencyformatter -->

#### NumberFormatter.decimalSeparator
<!-- id: N/format/i18n.NumberFormatter#decimalSeparator -->

`string`, read-only. The decimal separator the formatter uses. Since 2019.2.

> [!SOURCE-ERROR] As with `groupSeparator`, the source lists `CurrencyFormatter` as the parent
> object here although the property belongs to `NumberFormatter`. Verify on an instance.
<!-- source-error: N/format/i18n.NumberFormatter#decimalSeparator | - | parent_object_row_says_currencyformatter -->

#### NumberFormatter.precision
<!-- id: N/format/i18n.NumberFormatter#precision -->

`number`, read-only. The precision the formatter uses. Since 2019.2.

> [!SOURCE-ERROR] The source again names `CurrencyFormatter` as this property's parent object, while
> it is documented on `NumberFormatter`. Confirm on a real instance before depending on it.
<!-- source-error: N/format/i18n.NumberFormatter#precision | - | parent_object_row_says_currencyformatter -->

#### NumberFormatter.locale
<!-- id: N/format/i18n.NumberFormatter#locale -->

`string`, read-only. The locale the formatter uses. Since 2021.1.

### PhoneNumberFormatter (object)
<!-- id: N/format/i18n.PhoneNumberFormatter -->

Turns a phone number object into a string. You get one from
[`format.getPhoneNumberFormatter(options)`](#formatgetphonenumberformatteroptions). Client and
server scripts, since 2020.2.

#### PhoneNumberFormatter.format(options)
<!-- id: N/format/i18n.PhoneNumberFormatter#format -->

Writes a phone number object out as a string in the formatter's style.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.number` | Object | Yes | |

- `options.number`: a phone number object, such as the result of
  [`PhoneNumberParser.parse(options)`](#phonenumberparserparseoptions).

**Returns:** `String`, the formatted phone number.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2020.2

### PhoneNumberParser (object)
<!-- id: N/format/i18n.PhoneNumberParser -->

Turns a phone number string into a phone number object. You get one from
[`format.getPhoneNumberParser(options)`](#formatgetphonenumberparseroptions). Client and server
scripts, since 2020.2.

#### PhoneNumberParser.parse(options)
<!-- id: N/format/i18n.PhoneNumberParser#parse -->

Parses a phone number string. The country is taken from the country code in the input when there
is one; otherwise the parser's default country applies.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.number` | string | Yes | |

**Returns:** `Object`, a phone number object you can pass to
[`PhoneNumberFormatter.format(options)`](#phonenumberformatterformatoptions). The source's syntax
sample shows these fields on it: `countryCode`, `extension`, `nationalNumber`,
`numberOfLeadingZeros`, `carrierCode` and `rawInput`. It does not describe their types or meanings
further, and the list comes from a sample rather than a formal definition.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2020.2

## Enums

### format.Currency
<!-- id: N/format/i18n.format#Currency -->

Currency codes accepted by `options.currency` of
[`getCurrencyFormatter`](#formatgetcurrencyformatteroptions). Which values are available depends on
the company, so the list below is what the source records, not a guarantee for every account. The
source gives the codes only, with no further description. Since 2019.1.

| Value |
|---|
| `AED` |
| `CAD` |
| `CZK` |
| `EEK` |
| `EUR` |
| `GBP` |
| `IDR` |
| `INR` |
| `JPY` |
| `USD` |
| `UYU` |

### format.NegativeNumberFormat
<!-- id: N/format/i18n.format#NegativeNumberFormat -->

Chooses how a [`NumberFormatter`](#numberformatter-object) shows negative values; pass it as
`options.negativeNumberFormat` of [`getNumberFormatter`](#formatgetnumberformatteroptions).
Since 2019.2.

> [!SOURCE-ERROR] The source refers to the parameter that takes this enum as
> `negativeNumberFormatter`, while the documented parameter is `negativeNumberFormat`. Use
> `negativeNumberFormat` and verify it if the setting seems to be ignored.
<!-- source-error: N/format/i18n.format#NegativeNumberFormat | - | names_parameter_negativenumberformatter -->

The source lists the two values by name only and does not describe the output each produces.

| Value |
|---|
| `BRACKETS` |
| `MINUS` |

### format.PhoneNumberFormatType
<!-- id: N/format/i18n.format#PhoneNumberFormatType -->

Output styles for `options.formatType` of
[`getPhoneNumberFormatter`](#formatgetphonenumberformatteroptions). Since 2020.2. The source lists
the names only and does not describe what each style looks like; check the output in your account.

| Value |
|---|
| `E164` |
| `INTERNATIONAL` |
| `NATIONAL` |
| `RFC3966` |

### format.Country
<!-- id: N/format/i18n.format#Country -->

Countries accepted by `options.defaultCountry` of
[`getPhoneNumberParser`](#formatgetphonenumberparseroptions). Since 2020.2.

Some keys are misspelled in the source, and you must use them exactly as listed. The source's own
examples are marked in the table (`AMERICAN_SOMOA`, `GIBRALTER`, `SENGAL`, `NORFOLKISAND`,
`BOSIA_AND_HERZEGOVINA`); other odd spellings may exist.

| Value | Note |
|---|---|
| `AFGHANISTAN` | |
| `ALAND_ISLANDS` | |
| `ALBANIA` | |
| `ALGERIA` | |
| `AMERICAN_SOMOA` | misspelled key; use as listed |
| `ANDORRA` | |
| `ANGOLA` | |
| `ANGUILLA` | |
| `ANTARCTICA` | |
| `ANTIGUA_AND_BARBUDA` | |
| `ARGENTINA` | |
| `ARMENIA` | |
| `ARUBA` | |
| `AUSTRALIA` | |
| `AUSTRIA` | |
| `AZERBAIJAN` | |
| `BAHAMAS` | |
| `BAHRAIN` | |
| `BANGLADESH` | |
| `BARBADOS` | |
| `BELARUS` | |
| `BELGIUM` | |
| `BELIZE` | |
| `BENIN` | |
| `BERMUDA` | |
| `BHUTAN` | |
| `BOLIVIA` | |
| `BONAIRE` | |
| `BOSIA_AND_HERZEGOVINA` | misspelled key; use as listed |
| `BOTSWANA` | |
| `BOUVET_ISLAND` | |
| `BRAZIL` | |
| `BRITISH_INDIAN_OCEAN_TERRITORY` | |
| `BRUNEI_DARUSSALAM` | |
| `BULGARIA` | |
| `BURKINAFASO` | |
| `BURUNDI` | |
| `CAMBODIA` | |
| `CAMEROON` | |
| `CANADA` | |
| `CANARY_ISLANDS` | |
| `CAPEVERDE` | |
| `CAYMAN_ISLANDS` | |
| `CENTRAL_AFRICAN_REPUBLIC` | |
| `CEUTA_AND_MELILLA` | |
| `CHAD` | |
| `CHILE` | |
| `CHINA` | |
| `CHRISTMAS_ISLAND` | |
| `COCOS_ISLANDS` | |
| `COLOMBIA` | |
| `COMOROS` | |
| `COOK_ISLANDS` | |
| `COSTARICA` | |
| `COTE_DIVOIRE` | |
| `CROATIA` | |
| `CUBA` | |
| `CURACAO` | |
| `CYPRUS` | |
| `CZECH_REPUBLIC` | |
| `DEMOCRATIC_REPUBLIC_OF_CONGO` | |
| `DENMARK` | |
| `DJIBOUTI` | |
| `DOMINICA` | |
| `DOMINICAN_REPUBLIC` | |
| `EASTTIMOR` | |
| `ECUADOR` | |
| `EGYPT` | |
| `ELSALVADOR` | |
| `EQUATORIAL_GUINEA` | |
| `ERITREA` | |
| `ESTONIA` | |
| `ETHIOPIA` | |
| `FALKLAND_ISLANDS` | |
| `FAROE_ISLANDS` | |
| `FIJI` | |
| `FINLAND` | |
| `FRANCE` | |
| `FRENCH_POLYNESIA` | |
| `FRENCH_SOUTHERN_TERRITORIES` | |
| `FRENCHGUIANA` | |
| `GABON` | |
| `GAMBIA` | |
| `GEORGIA` | |
| `GERMANY` | |
| `GHANA` | |
| `GIBRALTER` | misspelled key; use as listed |
| `GREECE` | |
| `GREENLAND` | |
| `GRENADA` | |
| `GUADELOUPE` | |
| `GUAM` | |
| `GUATEMALA` | |
| `GUERNSEY` | |
| `GUINEA` | |
| `GUINEA_BISSAU` | |
| `GUYANA` | |
| `HAITI` | |
| `HEARD_AND_MCDONALD_ISLANDS` | |
| `HONDURAS` | |
| `HONGKONG` | |
| `HUNGARY` | |
| `ICELAND` | |
| `INDIA` | |
| `INDONESIA` | |
| `IRAN` | |
| `IRAQ` | |
| `IRELAND` | |
| `ISLEOFMAN` | |
| `ISRAEL` | |
| `ITALY` | |
| `JAMAICA` | |
| `JAPAN` | |
| `JERSEY` | |
| `JORDAN` | |
| `KAZAKHSTAN` | |
| `KENYA` | |
| `KIRIBATI` | |
| `KOREA_NORTH` | |
| `KOREA_SOUTH` | |
| `KOSOVO` | |
| `KUWAIT` | |
| `KYRGYZSTAN` | |
| `LAOS` | |
| `LATVIA` | |
| `LEBANON` | |
| `LESOTHO` | |
| `LIBERIA` | |
| `LIBYA` | |
| `LIECHTENSTEIN` | |
| `LITHUANIA` | |
| `LUXEMBOURG` | |
| `MACAU` | |
| `MACEDONIA` | |
| `MADAGASCAR` | |
| `MALAWI` | |
| `MALAYSIA` | |
| `MALDIVES` | |
| `MALI` | |
| `MALTA` | |
| `MARSHALL_ISLANDS` | |
| `MARTINIQUE` | |
| `MAURITANIA` | |
| `MAURITIUS` | |
| `MAYOTTE` | |
| `MEXICO` | |
| `MICRONESIA` | |
| `MOLDOVA` | |
| `MONACO` | |
| `MONGOLIA` | |
| `MONTENEGRO` | |
| `MONTSERRAT` | |
| `MOROCCO` | |
| `MOZAMBIQUE` | |
| `MYANMAR` | |
| `NAMIBIA` | |
| `NAURU` | |
| `NEPAL` | |
| `NETHERLANDS` | |
| `NETHERLANDS_ANTILLES` | |
| `NEWCALEDONIA` | |
| `NEWZEALAND` | |
| `NICARAGUA` | |
| `NIGER` | |
| `NIGERIA` | |
| `NIUE` | |
| `NORFOLKISAND` | misspelled key; use as listed |
| `NORTHERN_MARIANA_ISLANDS` | |
| `NORWAY` | |
| `OMAN` | |
| `PAKISTAN` | |
| `PALAU` | |
| `PANAMA` | |
| `PAPUA_NEW_GUINEA` | |
| `PARAGUAY` | |
| `PERU` | |
| `PHILIPPINES` | |
| `PITCAIRN_ISLAND` | |
| `POLAND` | |
| `PORTUGAL` | |
| `PUERTORICO` | |
| `QATAR` | |
| `REPUBLIC_OF_CONGO` | |
| `REUNION_ISLAND` | |
| `ROMANIA` | |
| `RUSSIAN_FEDERATION` | |
| `RWANDA` | |
| `SAINT_BARTHELEMY` | |
| `SAINT_HELENA` | |
| `SAINT_KITTS_AND_NEVIS` | |
| `SAINT_VICENT_AND_THE_GRENADINES` | |
| `SAINTLUCIA` | |
| `SAINTMARTIN` | |
| `SAMOA` | |
| `SANMARINO` | |
| `SAOTOME_AND_PRINCIPE` | |
| `SAUDI_ARABIA` | |
| `SENGAL` | misspelled key; use as listed |
| `SERBIA` | |
| `SERBIA_AND_MONTENEGRO` | |
| `SEYCHELLES` | |
| `SIERRALEONE` | |
| `SINGAPORE` | |
| `SINT_MAARTEN` | |
| `SLOVAK_REPUBLIC` | |
| `SLOVENIA` | |
| `SOLOMON_ISLANDS` | |
| `SOMALIA` | |
| `SOUTH_GEORGIA` | |
| `SOUTHAFRICA` | |
| `SOUTHSUDAN` | |
| `SPAIN` | |
| `SRILANKA` | |
| `ST_PIERREANDMIQUELON` | |
| `STATE_OF_PALESTINE` | |
| `SUDAN` | |
| `SURINAME` | |
| `SVALBARD_AND_JANMAYEN_ISLANDS` | |
| `SWAZILAND` | |
| `SWEDEN` | |
| `SWITZERLAND` | |
| `SYRIAN_ARAB_REPUBLIC` | |
| `TAIWAN` | |
| `TAJIKISTAN` | |
| `TANZANIA` | |
| `THAILAND` | |
| `TOGO` | |
| `TOKELAU` | |
| `TONGA` | |
| `TRINIDADANDTOBAGO` | |
| `TUNISIA` | |
| `TURKEY` | |
| `TURKMENISTAN` | |
| `TURKSAND_CAICOS_ISLANDS` | |
| `TUVALU` | |
| `UGANDA` | |
| `UKRAINE` | |
| `UNITED_ARAB_EMIRATES` | |
| `UNITED_KINGDOM` | |
| `UNITEDSTATES` | |
| `URUGUAY` | |
| `US_MINOR_OUTLYING_ISLANDS` | |
| `UZBEKISTAN` | |
| `VANUATU` | |
| `VATICAN` | |
| `VENEZUELA` | |
| `VIETNAM` | |
| `VIRGINISLANDS_UK` | |
| `VIRGINISLANDS_USA` | |
| `WALLIS_AND_FUTUNA` | |
| `WESTERN_SAHARA` | |
| `YEMEN` | |
| `ZAMBIA` | |
| `ZIMBABWE` | |

## Examples

A user event script on checks that writes the amount in German words into the memo, for a payee
who receives printed cheques in that language. `spellOut` costs nothing in governance.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/format/i18n'], (i18n) => {
  const beforeSubmit = (context) => {
    const chk = context.newRecord;
    const amount = Number(chk.getValue({ fieldId: 'usertotal' }));
    if (!amount) {
      return;
    }
    const words = i18n.spellOut({ locale: 'de', number: amount });
    chk.setValue({ fieldId: 'memo', value: `Betrag: ${words}` });
  };

  return { beforeSubmit };
});
```

A client script that tidies the mobile number on a contact as the user types it, assuming Irish
numbers when no country code is given and storing the result in international style. The parser
and formatter are built once in `pageInit`, so their 10-unit cost is paid once per page load.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/format/i18n'], (i18n) => {
  let parser = null;
  let formatter = null;

  const pageInit = () => {
    parser = i18n.getPhoneNumberParser({
      defaultCountry: i18n.Country.IRELAND
    });
    formatter = i18n.getPhoneNumberFormatter({
      formatType: i18n.PhoneNumberFormatType.INTERNATIONAL
    });
  };

  const fieldChanged = (context) => {
    if (context.fieldId !== 'mobilephone') {
      return;
    }
    const raw = context.currentRecord.getValue({ fieldId: 'mobilephone' });
    if (!raw) {
      return;
    }
    const tidy = formatter.format({ number: parser.parse({ number: raw }) });
    if (tidy !== raw) {
      context.currentRecord.setValue({ fieldId: 'mobilephone', value: tidy });
    }
  };

  return { pageInit, fieldChanged };
});
```

A RESTlet that returns a vendor bill total twice: once as a euro amount and once as a bare number
with bracketed negatives, using a number formatter with explicit separators.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/format/i18n', 'N/search'], (i18n, search) => {
  const get = (params) => {
    const fields = search.lookupFields({
      type: search.Type.VENDOR_BILL,
      id: params.billId,
      columns: ['total']
    });
    const total = Number(fields.total);

    const euros = i18n.getCurrencyFormatter({ currency: i18n.Currency.EUR });
    const plain = i18n.getNumberFormatter({
      groupSeparator: ' ',
      decimalSeparator: ',',
      precision: 2,
      negativeNumberFormat: i18n.NegativeNumberFormat.BRACKETS
    });

    return {
      billId: params.billId,
      display: euros.format({ number: total }),
      ledger: plain.format({ number: total }),
      symbol: euros.symbol
    };
  };

  return { get };
});
```
