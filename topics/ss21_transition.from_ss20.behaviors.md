# SuiteScript 2.0 vs 2.1 runtime behavior
<!-- id: topic:ss21_transition.from_ss20.behaviors -->

Part of: [SuiteScript 2.0 to 2.1 differences](ss21_transition.from_ss20.md)

2.0 scripts run on an ECMAScript 5.1 engine; 2.1 scripts run on ECMAScript 2023. Code that the old
engine tolerated can fail or behave differently on the new one. The cases below are the ones to
look for when moving a script.

## Summary

| Code pattern | On 2.1 |
|---|---|
| `JSON.parse` of text with a trailing comma | `SyntaxError` |
| Any `JSON.parse` failure | The error message is worded differently, so code that inspects it may break |
| A variable or function named with a reserved word such as `extends` | Syntax error |
| `JSON.stringify` of an `Error` | Only enumerable properties appear; `message` is missing |
| Assigning to an undeclared variable | Throws (strict mode) |
| Reassigning a `const` | `TypeError` |
| `for each (... in ...)` | `SyntaxError` |
| Conditional `catch (e if ...)` blocks | `SyntaxError` |
| `toSource()` | Throws |
| `toLocaleDateString()` with no options | Returns the short date format |

## JSON

Strict JSON only: a trailing comma in an object or array makes parsing fail. If your code matches
on the text of parse errors, rework it, because the message format has changed.

## Reserved words

Words such as `extends` cannot be used as variable or function names. Avoid every ECMAScript
reserved word as an identifier, including ones you have not hit yet.

## Error objects

`JSON.stringify` only writes an object's enumerable properties. The `message` that the `Error`
constructor sets is not enumerable, so stringifying an error no longer shows what went wrong. Copy
the fields you want into a plain object first.

## Strict mode and const

- Assigning to a variable that was never declared throws instead of creating a global.
- Assigning a new value to a `const` throws a `TypeError`.

## Syntax that no longer works

`for each...in` loops and conditional catch clauses are rejected as syntax errors, and calling
`toSource` throws. (As general JavaScript background: none of the three is part of standard
ECMAScript.)

## Dates

Called without arguments, `toLocaleDateString` returns a short date. If a script depends on a
particular format, pass explicit options or use [N/format](../modules/N_format.md).

The source illustrates this with a sample that has three problems of its own:

- It builds the date with month `12`, which JavaScript (months count from 0) rolls over to January
  of the following year (2013), not December.

> [!SOURCE-ERROR] The sample passes 12 as the month and treats it as December; JavaScript reads it
> as January 2013. Use 11 for December when you adapt the sample.
<!-- source-error: topic:ss21_transition.from_ss20.behaviors | - | sample_month_12_means_january_2013_not_december -->

- It calls `toLocalDateString` [sic], a misspelling of `toLocaleDateString`.

> [!SOURCE-ERROR] The method name in the sample is missing an "e"; no such method exists, so the
> sample would throw. The real method is `toLocaleDateString`.
<!-- source-error: topic:ss21_transition.from_ss20.behaviors | - | sample_calls_misspelled_toLocalDateString -->

- It is missing a closing parenthesis, so it does not parse as printed.

> [!SOURCE-ERROR] The sample is not valid JavaScript as given because a closing parenthesis is
> missing. Do not copy it; write the call out yourself.
<!-- source-error: topic:ss21_transition.from_ss20.behaviors | - | sample_missing_closing_parenthesis -->

## Example

A 2.1 RESTlet that avoids three of the traps above: it parses JSON defensively, logs errors as plain
objects so their message survives `JSON.stringify`, and formats a date with explicit options.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/log'], (log) => {
  const describeError = (e) => ({ name: e.name, message: e.message });

  const post = (body) => {
    let payload;
    try {
      payload = typeof body === 'string' ? JSON.parse(body) : body;
    } catch (e) {
      log.error({ title: 'Bad JSON', details: JSON.stringify(describeError(e)) });
      return JSON.stringify({ ok: false, reason: 'Request body is not valid JSON' });
    }
    const receivedOn = new Date().toLocaleDateString('en-US', {
      year: 'numeric', month: 'long', day: 'numeric'
    });
    return JSON.stringify({ ok: true, keys: Object.keys(payload), receivedOn });
  };

  return { post };
});
```
