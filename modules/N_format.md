# N/format
<!-- id: N/format -->

`N/format` translates values between their raw form (a JavaScript `Date`, a number) and the
formatted strings a user sees, according to that user's preferences: date format, number format,
time zone and so on. Users choose their personal formats under Home > Set Preferences, and both
functions here follow those settings.

- [`format.parse`](#formatparseoptions): user-formatted string to raw value.
- [`format.format`](#formatformatoptions): raw value to user-formatted string.
- **Availability:** client and server scripts.
- **Load it as:** `define(['N/format'], (format) => { ... })`
- Both functions cost 0 governance units and are available since 2015.2.
- Related: [N/format/i18n](N_format_i18n.md) for locale-based number, currency and phone formatting.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`format.parse(options)`](#formatparseoptions) | function | Turn a preference-formatted string into a raw value |
| [`format.format(options)`](#formatformatoptions) | function | Turn a raw value into a preference-formatted string |
| [`format.Type`](#formattype) | enum | Field types that drive the conversion |
| [`format.Timezone`](#formattimezone) | enum | Olson time zones for date/time conversions |

## Functions

### format.parse(options)
<!-- id: N/format.parse -->

Reads a string written in the user's preferred format and gives back the raw value. For the
`DATETIME` and `DATETIMETZ` types the result is a `Date`, and this is also where the optional
`timezone` comes into play (the function has a separate overload for those two types). The expected
input format comes from the user's date format preference, which is set in Set Preferences.

If the string cannot be parsed or is not valid for the type, you get the input back unchanged
rather than an error, so check the result's type before trusting it (for example
`result instanceof Date`).

Where the time comes from depends on where the script runs: in a client script it is based on the
user's own system time; on the server it is based on Pacific time, with daylight saving applied.

> [!SOURCE-ERROR] The statement that server-side parsing is based on Pacific time with daylight
> saving is flagged as doubtful: the source's companion description of `format.format` bases
> server-side strings on the user's or company's time zone instead. The spec does not settle which
> is right; pass `timezone` explicitly when the zone matters and test the result on the server.
<!-- source-error: N/format.parse | - | server_string_based_on_pacific_time_with_dst -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.timezone` | enum | No | |
| `options.value` | string | Yes | |

- `options.type`: a [`format.Type`](#formattype) value. Since 2015.2.
- `options.timezone`: only used with `DATETIME` and `DATETIMETZ`. It names the time zone the input
  string is expressed in, as a [`format.Timezone`](#formattimezone) value. When omitted, the time
  zone from the user's preferences is used; an invalid time zone falls back to GMT. Since 2015.2.
- `options.value`: the formatted string to parse. Since 2015.2.

**Returns:** `Date | string`. A `Date` for `DATETIME` / `DATETIMETZ`; otherwise the raw value; the
original input if it could not be parsed.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### format.format(options)
<!-- id: N/format.format -->

Produces the string a user would see for a raw value, using their preferences. For `DATETIME` and
`DATETIMETZ` (which have their own overload) the result is a date/time string in the user's local
time zone, and you can pass `timezone` to choose a different one. An invalid value is returned
unchanged instead of raising an error.

Which time zone applies by default depends on where the script runs: in a client script, the user's
system time; on the server, the user's or the company's time zone.

> [!SOURCE-ERROR] The sources disagree about server-side behaviour: this page bases the server string
> on the user's or company's time zone, while another passage says the server uses Pacific time with
> daylight saving. The spec keeps this page's value without deciding which is right; if the exact
> zone matters, pass `timezone` explicitly rather than relying on the default.
<!-- source-error: N/format.format | - | server_string_based_on_pacific_time_with_dst -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.timezone` | enum \| number | No | |
| `options.value` | Date \| string \| number | Yes | |

- `options.type`: a [`format.Type`](#formattype) value. Since 2015.2.
- `options.timezone`: only used with `DATETIME` and `DATETIMETZ`. Accepts a
  [`format.Timezone`](#formattimezone) value or the time zone's numeric key. When omitted, the
  user's preference applies; an invalid time zone falls back to GMT. Since 2015.2.
- `options.value`: the raw value to format. Since 2015.2.

**Returns:** `string`, the value formatted per the user's preferences (or the input unchanged if it
was invalid).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

## Enums

### format.Type
<!-- id: N/format.Type -->

The field type that tells [`format.parse`](#formatparseoptions) and
[`format.format`](#formatformatoptions) how to interpret or present a value. Since 2015.2. The source
gives these rules for particular types:

- `POSCURRENCY`, `POSFLOAT` and `POSINTEGER` accept only values greater than 0.
- `NONNEGFLOAT` accepts values of 0 or more.
- `CURRENCY` is rounded to the user's currency precision, with at most 2 decimal places.
- `CURRENCY2` uses the precision of the record's currency instead.
- Hidden, `CHECKBOX`, `RADIO`, `SELECT` and `TEXTAREA` fields come back as text. (The list below has
  no separate hidden value, so it is unclear which value that statement refers to.)
- `DYNAMICPRECISION`: NetSuite decides the precision for each field.

Apart from these rules, the source does not describe what each value means or how it formats; the
names are all it gives. Test a value before relying on its output.

> [!SOURCE-ERROR] The source misspells the select type as `seelct` [sic] in its description. The
> enum value itself is listed as `SELECT`; use that name.
<!-- source-error: N/format.Type | - | select_misspelled_seelct -->

All 44 values (names only):

| Value |
|---|
| `ADDRESS` |
| `EMAILS` |
| `POSCURRENCY` |
| `CCEXPDATE` |
| `FLOAT` |
| `POSFLOAT` |
| `CCNUMBER` |
| `FULLPHONE` |
| `POSINTEGER` |
| `CCVALIDFROM` |
| `FUNCTION` |
| `QUOTEDFUNCTION` |
| `CHECKBOX` |
| `FURIGANA` |
| `RADIO` |
| `CLOBTEXT` |
| `IDENTIFIER` |
| `RATE` |
| `COLOR` |
| `IDENTIFIERANYCASE` |
| `RATEHIGHPRECISION` |
| `CURRENCY` |
| `INTEGER` |
| `SELECT` |
| `CURRENCY2` |
| `MULTISELECT` |
| `TEXT` |
| `DATE` |
| `MMYYDATE` |
| `TEXTAREA` |
| `DATETIME` |
| `NONNEGCURRENCY` |
| `TIME` |
| `DATETIMETZ` |
| `NONNEGFLOAT` |
| `TIMEOFDAY` |
| `DOCUMENT` |
| `PACKAGE` |
| `TIMETRACK` |
| `DYNAMICPRECISION` |
| `PERCENT` |
| `URL` |
| `EMAIL` |
| `PHONE` |

### format.Timezone
<!-- id: N/format.Timezone -->

Time zones for the `timezone` option of [`format.parse`](#formatparseoptions) and
[`format.format`](#formatformatoptions). Each value is an Olson (IANA) time zone string, and the
list is the same as the time zone list in Set Preferences. Since 2015.2.

You can pass either the enum value (its Olson string) or the zone's numeric key. The numeric key is
the form to use when the time zone comes from a custom time-zone dropdown field. The source's own
examples are key 5 for `America/Los_Angeles`, 15 for `America/New_York` and 32 for `Europe/London`;
the table below gives every key. Avoid key 9 (`America/Chihuahua`): the source advises against it
without giving a reason.

> [!SOURCE-ERROR] The source misspells the description of the Guadalcanal zone. Rely on the Olson
> value `Pacific/Guadalcanal` shown in the table rather than the source's wording.
<!-- source-error: N/format.Timezone | - | guadalcanal_description_misspelled -->

The key name `PACIFIC_GUADALCANA` [sic] is kept exactly as the source spells it.

> [!SOURCE-ERROR] The key `PACIFIC_GUADALCANA` [sic] looks truncated compared with its Olson value
> `Pacific/Guadalcanal`. Use the name as listed (or pass the numeric key 84 instead), and verify it
> in your account before relying on it.
<!-- source-error: N/format.Timezone | PACIFIC_GUADALCANA | key_name_pacific_guadalcana_looks_truncated -->

All 88 values:

| Value | Key | Olson time zone |
|---|---|---|
| `ETC_GMT_PLUS_12` | 1 | `Etc/GMT+12` |
| `PACIFIC_SAMOA` | 2 | `Pacific/Samoa` |
| `PACIFIC_HONOLULU` | 3 | `Pacific/Honolulu` |
| `AMERICA_ANCHORAGE` | 4 | `America/Anchorage` |
| `AMERICA_LOS_ANGELES` | 5 | `America/Los_Angeles` |
| `AMERICA_TIJUANA` | 6 | `America/Tijuana` |
| `AMERICA_DENVER` | 7 | `America/Denver` |
| `AMERICA_PHOENIX` | 8 | `America/Phoenix` |
| `AMERICA_CHIHUAHUA` | 9 | `America/Chihuahua` |
| `AMERICA_HERMOSILLO` | 10 | `America/Hermosillo` |
| `AMERICA_CHICAGO` | 11 | `America/Chicago` |
| `AMERICA_REGINA` | 12 | `America/Regina` |
| `AMERICA_GUATEMALA` | 13 | `America/Guatemala` |
| `AMERICA_MEXICO_CITY` | 14 | `America/Mexico_City` |
| `AMERICA_NEW_YORK` | 15 | `America/New_York` |
| `US_EAST_INDIANA` | 16 | `US/East-Indiana` |
| `AMERICA_BOGOTA` | 17 | `America/Bogota` |
| `AMERICA_CARACAS` | 18 | `America/Caracas` |
| `AMERICA_HALIFAX` | 19 | `America/Halifax` |
| `AMERICA_LA_PAZ` | 20 | `America/La_Paz` |
| `AMERICA_MANAUS` | 21 | `America/Manaus` |
| `AMERICA_SANTIAGO` | 22 | `America/Santiago` |
| `AMERICA_ST_JOHNS` | 23 | `America/St_Johns` |
| `AMERICA_SAO_PAULO` | 24 | `America/Sao_Paulo` |
| `AMERICA_BUENOS_AIRES` | 25 | `America/Buenos_Aires` |
| `ETC_GMT_PLUS_3` | 26 | `Etc/GMT+3` |
| `AMERICA_GODTHAB` | 27 | `America/Godthab` |
| `AMERICA_MONTEVIDEO` | 28 | `America/Montevideo` |
| `AMERICA_NORONHA` | 29 | `America/Noronha` |
| `ETC_GMT_PLUS_1` | 30 | `Etc/GMT+1` |
| `ATLANTIC_AZORES` | 31 | `Atlantic/Azores` |
| `EUROPE_LONDON` | 32 | `Europe/London` |
| `GMT` | 33 | `GMT` |
| `ATLANTIC_REYKJAVIK` | 34 | `Atlantic/Reykjavik` |
| `EUROPE_WARSAW` | 35 | `Europe/Warsaw` |
| `EUROPE_PARIS` | 36 | `Europe/Paris` |
| `ETC_GMT_MINUS_1` | 37 | `Etc/GMT-1` |
| `EUROPE_AMSTERDAM` | 38 | `Europe/Amsterdam` |
| `EUROPE_BUDAPEST` | 39 | `Europe/Budapest` |
| `AFRICA_CAIRO` | 40 | `Africa/Cairo` |
| `EUROPE_ISTANBUL` | 41 | `Europe/Istanbul` |
| `ASIA_JERUSALEM` | 42 | `Asia/Jerusalem` |
| `ASIA_AMMAN` | 43 | `Asia/Amman` |
| `ASIA_BEIRUT` | 44 | `Asia/Beirut` |
| `AFRICA_JOHANNESBURG` | 45 | `Africa/Johannesburg` |
| `EUROPE_KIEV` | 46 | `Europe/Kiev` |
| `EUROPE_MINSK` | 47 | `Europe/Minsk` |
| `AFRICA_WINDHOEK` | 48 | `Africa/Windhoek` |
| `ASIA_RIYADH` | 49 | `Asia/Riyadh` |
| `EUROPE_MOSCOW` | 50 | `Europe/Moscow` |
| `ASIA_BAGHDAD` | 51 | `Asia/Baghdad` |
| `AFRICA_NAIROBI` | 52 | `Africa/Nairobi` |
| `ASIA_TEHRAN` | 53 | `Asia/Tehran` |
| `ASIA_MUSCAT` | 54 | `Asia/Muscat` |
| `ASIA_BAKU` | 55 | `Asia/Baku` |
| `ASIA_YEREVAN` | 56 | `Asia/Yerevan` |
| `ETC_GMT_MINUS_3` | 57 | `Etc/GMT-3` |
| `ASIA_KABUL` | 58 | `Asia/Kabul` |
| `ASIA_KARACHI` | 59 | `Asia/Karachi` |
| `ASIA_YEKATERINBURG` | 60 | `Asia/Yekaterinburg` |
| `ASIA_TASHKENT` | 61 | `Asia/Tashkent` |
| `ASIA_CALCUTTA` | 62 | `Asia/Calcutta` |
| `ASIA_KATMANDU` | 63 | `Asia/Katmandu` |
| `ASIA_ALMATY` | 64 | `Asia/Almaty` |
| `ASIA_DHAKA` | 65 | `Asia/Dhaka` |
| `ASIA_RANGOON` | 66 | `Asia/Rangoon` |
| `ASIA_BANGKOK` | 67 | `Asia/Bangkok` |
| `ASIA_KRASNOYARSK` | 68 | `Asia/Krasnoyarsk` |
| `ASIA_HONG_KONG` | 69 | `Asia/Hong_Kong` |
| `ASIA_KUALA_LUMPUR` | 70 | `Asia/Kuala_Lumpur` |
| `ASIA_TAIPEI` | 71 | `Asia/Taipei` |
| `AUSTRALIA_PERTH` | 72 | `Australia/Perth` |
| `ASIA_IRKUTSK` | 73 | `Asia/Irkutsk` |
| `ASIA_MANILA` | 74 | `Asia/Manila` |
| `ASIA_SEOUL` | 75 | `Asia/Seoul` |
| `ASIA_TOKYO` | 76 | `Asia/Tokyo` |
| `ASIA_YAKUTSK` | 77 | `Asia/Yakutsk` |
| `AUSTRALIA_DARWIN` | 78 | `Australia/Darwin` |
| `AUSTRALIA_ADELAIDE` | 79 | `Australia/Adelaide` |
| `AUSTRALIA_SYDNEY` | 80 | `Australia/Sydney` |
| `AUSTRALIA_BRISBANE` | 81 | `Australia/Brisbane` |
| `AUSTRALIA_HOBART` | 82 | `Australia/Hobart` |
| `PACIFIC_GUAM` | 83 | `Pacific/Guam` |
| `PACIFIC_GUADALCANA` [sic] | 84 | `Pacific/Guadalcanal` |
| `ASIA_VLADIVOSTOK` | 85 | `Asia/Vladivostok` |
| `PACIFIC_KWAJALEIN` | 86 | `Pacific/Kwajalein` |
| `PACIFIC_AUCKLAND` | 87 | `Pacific/Auckland` |
| `PACIFIC_TONGATAPU` | 88 | `Pacific/Tongatapu` |

## Examples

A RESTlet that accepts a requested ship date typed by a user in their own date format, converts it
to a `Date`, and writes it to a sales order. Because `parse` hands back the input unchanged when it
cannot read it, the result is checked before use.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/format', 'N/record'], (format, record) => {
  const put = (body) => {
    const shipDate = format.parse({ value: body.requestedShipDate, type: format.Type.DATE });

    if (!(shipDate instanceof Date)) {
      return { updated: false, reason: `Could not read "${body.requestedShipDate}" as a date` };
    }

    record.submitFields({
      type: record.Type.SALES_ORDER,
      id: body.orderId,
      values: { shipdate: shipDate }
    });
    return { updated: true };
  };

  return { put };
});
```

Showing the same moment in a warehouse's local time and in the user's own format. Passing
`timezone` explicitly avoids depending on the server-side default, which the source describes
inconsistently.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/format'], (format) => {
  const describeCutoff = (cutoff) => {
    const warehouseTime = format.format({
      value: cutoff,
      type: format.Type.DATETIMETZ,
      timezone: format.Timezone.AMERICA_CHICAGO
    });
    const userTime = format.format({ value: cutoff, type: format.Type.DATETIME });
    return `Orders placed before ${warehouseTime} (Chicago) / ${userTime} (your time) ship today`;
  };

  return { describeCutoff };
});
```

Formatting an amount for a client-side confirmation. `CURRENCY` is rounded to the user's currency
precision (at most two decimals).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/format'], (format) => {
  const saveRecord = (context) => {
    const rec = context.currentRecord;
    const total = rec.getValue({ fieldId: 'total' });
    const shown = format.format({ value: total, type: format.Type.CURRENCY });
    return window.confirm(`Submit this order for ${shown}?`);
  };

  return { saveRecord };
});
```
