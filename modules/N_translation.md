# N/translation
<!-- id: N/translation -->

`N/translation` reads strings from Translation Collections, so a script can show text in the
user's language instead of hard-coding it. Access is read-only: collections and their strings are
created and edited in the UI (Customization > Translations > Manage Translations), not from script.

A translated string may contain numbered placeholders. When you call the translator function you
get back, it fills them with the values you pass: a string "Hello, {1}" given the value "NetSuite"
becomes "Hello, NetSuite". You can also load collections in locales other than the current one and
switch between them.

Templates have matching operations, `nstranslation.get` and `nstranslation.load`.

- **Availability:** client and server scripts.
- **Load it as:** `define(['N/translation'], (translation) => { ... })`
- **Since:** 2019.1

## Members

| Member | Kind | Summary |
|---|---|---|
| [`translation.get(options)`](#translationgetoptions) | function | Translator for one key |
| [`translation.load(options)`](#translationloadoptions) | function | Handle with chosen keys from one or more collections |
| [`translation.selectLocale(options)`](#translationselectlocaleoptions) | function | Same handle, different locale |
| [`Handle`](#handle-object) | object | Tree of loaded strings |
| [`Translator`](#translator-object) | object | Function that returns one translated string |
| [`translation.Locale`](#translationlocale) | enum | Locale codes |

## Functions

### translation.get(options)
<!-- id: N/translation.get -->

Returns a [`Translator`](#translator-object) for a single key of a collection; call the translator
to obtain the string. Template counterpart: `nstranslation.get`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.locale` | string | No | |
| `options.collection` | string | Yes | |
| `options.key` | string | Yes | |

- `options.locale`: a [`translation.Locale`](#translationlocale) value. When omitted, the
  session's locale is used.
- `options.collection`: the Translation Collection to read from.
- `options.key`: the string's key in that collection.

**Returns:** `translation.Translator`, a function that produces the translated string.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: the collection or the key was not given.
- `INVALID_TRANSLATION_KEY`: the key is not in a valid format.
- `INVALID_TRANSLATION_COLLECTION`: the collection is not in a valid format.
- `INVALID_LOCALE`: the locale is not in a valid format.
- `TRANSLATION_KEY_NOT_FOUND`: the key does not exist in the collection.

**Governance:** 1 unit.

**Availability:** client and server scripts.

**Since:** 2019.1

### translation.load(options)
<!-- id: N/translation.load -->

Loads selected strings from one or more collections, optionally in several locales, and returns a
[`Handle`](#handle-object) organised by collection alias and key. You must list the keys you want;
a whole collection cannot be loaded in one go. One call can load at most 1,000 strings, counted
across all its collections. Template counterpart: `nstranslation.load`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.locales` | string[] | No | |
| `options.collections` | Object[] | Yes | |
| `options.collections.alias` | string | Yes | |
| `options.collections.collection` | string | Yes | |
| `options.collections.keys` | string[] | Yes | |

- `options.locales`: [`translation.Locale`](#translationlocale) values. The returned handle uses
  the first locale listed; the others are loaded so you can switch to them later with
  `selectLocale`, which only works for locales loaded here.
- `options.collections.alias`: the name under which this collection appears on the handle.
- `options.collections.collection`: the collection to read.
- `options.collections.keys`: the keys to load from it.

**Returns:** `translation.Handle`, organised by alias and key.

**Errors:**

- `WRONG_PARAMETER_TYPE`: `options.collections`, `options.collections.keys` or `options.locales`
  is not an array.
- `SSS_MISSING_REQD_ARGUMENT`: a collection or a key is missing.
- `INVALID_TRANSLATION_KEY`: a key is not in a valid format.
- `INVALID_TRANSLATION_COLLECTION`: a collection is not in a valid format.
- `INVALID_LOCALE`: a locale is not in a valid format.
- `INVALID_ALIAS`: an alias is not in a valid format.

**Governance:** 10 units. Another source gives 1 unit; the spec does not say which is right.

> [!SOURCE-ERROR] The sources disagree on this call's cost: the main figure is 10 units, while a
> second source says 1 unit. Nothing settles which is correct, so budget for the higher figure.
<!-- source-error: N/translation.load | - | governance_sources_disagree -->

**Availability:** client and server scripts.

**Since:** 2019.1

### translation.selectLocale(options)
<!-- id: N/translation.selectLocale -->

Returns a handle holding the same strings as an existing one, but in another locale. That locale
must have been included in `options.locales` when the handle was built with
[`load`](#translationloadoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.locale` | string | Yes | |
| `options.handle` | translation.Handle | Yes | |

- `options.locale`: a [`translation.Locale`](#translationlocale) value.
- `options.handle`: a handle returned by `load` (or by an earlier `selectLocale`).

**Returns:** `translation.Handle`, the same strings in the requested locale.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: the handle or the locale was not given.
- `WRONG_PARAMETER_TYPE`: `options.handle` is not a translation handle.
- `INVALID_LOCALE`: the handle does not support that locale (it was not loaded).
- `TRANSLATION_HANDLE_IS_IN_AN_ILLEGAL_STATE`: the handle is in an illegal state.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2019.1

## Objects

### Handle (object)
<!-- id: N/translation.Handle -->

The strings of one or more collections in one locale, arranged as a tree: each node is either
another handle (for example, one per collection alias) or a [`Translator`](#translator-object)
(one per key). Returned by `load` and `selectLocale`. The source lists no named properties or
methods for it beyond this alias-and-key structure. Client and server scripts, since 2019.1.

### Translator (object)
<!-- id: N/translation.Translator -->

A function that returns the translated string for one key. Placeholders inside the string are
numbered braces (`{1}`, `{2}`, ...). To fill them, call the translator with an options object
whose optional `params` property is an array of strings; the first entry replaces `{1}`, and so
on. If `params` is not an array, the call raises `WRONG_PARAMETER_TYPE`. For example, a string
"Hello, {1}" called with `params: ['NetSuite']` yields "Hello, NetSuite". The strings themselves
are maintained in the Translation Collection UI. Returned by `get` and found at the leaves of a
`Handle`. Client and server scripts, since 2019.1.

## Enums

### translation.Locale
<!-- id: N/translation.Locale -->

Locale identifiers for `get`, `load` and `selectLocale`. Which locales actually work depends on the
languages enabled for the company. Two values are special: `CURRENT` stands for the current
user's locale and `COMPANY_DEFAULT` for the company's default locale; the rest are locale codes.
Since 2019.1.

| Value | Value | Value | Value |
|---|---|---|---|
| `CURRENT` | `COMPANY_DEFAULT` | `af_ZA` | `ar` |
| `bg_BG` | `bn_BD` | `bs_BA` | `cs_CZ` |
| `da_DK` | `de_DE` | `el_GR` | `en` |
| `en_AU` | `en_CA` | `en_GB` | `en_US` |
| `es_AR` | `es_ES` | `et_EE` | `fa_IR` |
| `fi_FI` | `fr_CA` | `fr_FR` | `gu_IN` |
| `he_IL` | `hi_IN` | `hr_HR` | `hu_HU` |
| `hy_AM` | `id_ID` | `is_IS` | `it_IT` |
| `ja_JP` | `kn_IN` | `ko_KR` | `lb_LU` |
| `lt_LT` | `lv_LV` | `mr_IN` | `ms_MY` |
| `nl_NL` | `no_NO` | `pa_IN` | `pl_PL` |
| `pt_BR` | `pt_PT` | `ro_RO` | `ru_RU` |
| `sh_RS` | `sk_SK` | `sl_SI` | `sq_AL` |
| `sr_RS` | `sv_SE` | `ta_IN` | `te_IN` |
| `th_TH` | `tl_PH` | `tr_TR` | `uk_UA` |
| `vi_VN` | `zh_CN` | `zh_TW` | |

## Examples

A client script that warns about a low credit limit in the user's own language. The collection
`custcollection_ar_messages` and its key `LOW_CREDIT` (text along the lines of "Only {1} of
credit remains") are invented for the example.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/translation', 'N/ui/dialog'], (translation, dialog) => {
  const pageInit = () => {};

  const saveRecord = (context) => {
    const cust = context.currentRecord;
    const limit = Number(cust.getValue({ fieldId: 'creditlimit' })) || 0;
    if (limit > 0 && limit < 1000) {
      const lowCredit = translation.get({
        collection: 'custcollection_ar_messages',
        key: 'LOW_CREDIT'
      });
      dialog.alert({ title: 'Credit', message: lowCredit({ params: [String(limit)] }) });
    }
    return true;
  };

  return { pageInit, saveRecord };
});
```

A Suitelet that loads two labels in French and German at once, then switches the handle to German
for the second column of a bilingual packing slip header:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/translation'], (translation) => {
  const onRequest = (context) => {
    const fr = translation.load({
      locales: [translation.Locale.fr_FR, translation.Locale.de_DE],
      collections: [{
        alias: 'slip',
        collection: 'custcollection_packing_slip',
        keys: ['SHIP_TO', 'CARTON_COUNT']
      }]
    });
    const de = translation.selectLocale({ handle: fr, locale: translation.Locale.de_DE });

    const header = ['SHIP_TO', 'CARTON_COUNT']
      .map((key) => `${fr.slip[key]()} / ${de.slip[key]()}`)
      .join('\n');
    context.response.write({ output: header });
  };

  return { onRequest };
});
```
