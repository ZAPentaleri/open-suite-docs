# N/ui/message
<!-- id: N/ui/message -->

`N/ui/message` puts a coloured banner across the page, just below the menu bar, to tell the user
something. You create a message object, then show it and hide it as needed. The banner's type sets
its background colour and icon.

Use this module instead of editing the NetSuite page through the DOM; scripts cannot access the
NetSuite UI that way and should go through the SuiteScript APIs.

- **Availability:** client scripts only.
- **Load it as:** `define(['N/ui/message'], (message) => { ... })`
- Related: [`Form.addPageInitMessage`](N_ui_serverWidget.md#formaddpageinitmessageoptions) in
  N/ui/serverWidget queues a message of this kind from the server side.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`message.create(options)`](#messagecreateoptions) | function | Build a banner |
| [`Message`](#message-object) | object | A banner you can show and hide |
| [`message.Type`](#messagetype) | enum | Banner style |

## Functions

### message.create(options)
<!-- id: N/ui/message.create -->

Creates a banner that can be shown or hidden near the top of the page. Creating it does not display
it; call [`Message.show`](#messageshowoptions) for that.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | message.Type | Yes | |
| `options.title` | string | No | `""` |
| `options.message` | string | No | `""` |
| `options.duration` | int \| string | No | `0` |

- `options.type`: a [`message.Type`](#messagetype) value. Since 2016.1.
- `options.title`: the banner heading. Since 2016.1.
- `options.message`: the banner text. Since 2016.1.
- `options.duration`: how long the banner stays visible once shown, in milliseconds, as a number or
  a numeric string. `0` keeps it up until `hide` is called. A duration passed to `show` takes
  precedence over this one. Since 2018.2.

> [!SOURCE-ERROR] The source's text for this parameter repeats a method name (it mentions `hide`
> where a different method is presumably meant), so that sentence is garbled. The spec flags it for
> human review; rely on the behaviour described above.
<!-- source-error: N/ui/message.create | options.duration | method_name_repeated_message_hide -->

**Returns:** `message.Message`, the banner object.

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

## Objects

### Message (object)
<!-- id: N/ui/message.Message -->

A banner produced by [`message.create`](#messagecreateoptions). Client scripts, since 2016.1.

#### Message.show(options)
<!-- id: N/ui/message.Message#show -->

Displays the banner.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.duration` | int \| string | No | `0` |

- `options.duration`: milliseconds to keep the banner visible, as a number or numeric string; `0`
  means it stays until `hide` is called. When given, it overrides the duration set in `create`.
  Since 2016.1.

**Returns:** `void`

**Errors:**

- `WRONG_PARAMETER_TYPE`: `options.duration` has the wrong type, that is, it is neither a number
  nor a string.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

#### Message.hide()
<!-- id: N/ui/message.Message#hide -->

Removes the banner from the page. Takes no parameters.

**Returns:** `void`

**Errors:** the source does not state any.

**Governance:** 0 units.

**Availability:** client scripts.

**Since:** 2016.1

## Enums

### message.Type
<!-- id: N/ui/message.Type -->

The banner style, which decides its background colour and icon. Used by `options.type` of
[`message.create`](#messagecreateoptions). Since 2016.1.

| Value | Appearance |
|---|---|
| `CONFIRMATION` | Green, with a check mark |
| `INFORMATION` | Blue, with an information icon |
| `WARNING` | Yellow, with a warning icon |
| `ERROR` | Red, with an X icon |

> [!SOURCE-ERROR] In the source's table for this enum, a column heading is misspelled
> ("characteristcs" [sic]). It is a cosmetic slip and does not affect the values; the spec marks it
> for review.
<!-- source-error: N/ui/message.Type | - | column_header_misspelled_characteristcs -->

## Examples

A client script on sales orders that shows a warning while the chosen ship date falls on a weekend,
and clears it once the date is fixed.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/message'], (message) => {
  const weekendWarning = message.create({
    type: message.Type.WARNING,
    title: 'Weekend ship date',
    message: 'The warehouse does not dispatch on Saturdays or Sundays.'
  });

  const pageInit = () => {};

  const fieldChanged = (context) => {
    if (context.fieldId !== 'shipdate') {
      return;
    }
    const shipDate = context.currentRecord.getValue({ fieldId: 'shipdate' });
    const day = shipDate instanceof Date ? shipDate.getDay() : -1;
    if (day === 0 || day === 6) {
      weekendWarning.show();
    } else {
      weekendWarning.hide();
    }
  };

  return { pageInit, fieldChanged };
});
```

A short-lived confirmation when the page opens, using the duration on `show` (which overrides any
duration given to `create`):

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/message'], (message) => {
  const pageInit = () => {
    const ready = message.create({
      type: message.Type.CONFIRMATION,
      title: 'Price list loaded',
      message: 'Customer-specific prices are applied to new lines.',
      duration: 20000
    });
    ready.show({ duration: 4000 });
  };

  return { pageInit };
});
```
