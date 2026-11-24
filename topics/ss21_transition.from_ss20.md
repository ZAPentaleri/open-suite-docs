# SuiteScript 2.0 to 2.1 differences
<!-- id: topic:ss21_transition.from_ss20 -->

Part of: [Transitioning to SuiteScript 2.1](ss21_transition.md)

Sub-topics:

- [SuiteScript 2.0 vs 2.1 runtime behavior](ss21_transition.from_ss20.behaviors.md)
- [SuiteScript 2.0 vs 2.1 integration behavior](ss21_transition.from_ss20.integration.md)

## Same API, different engine

The modules and their members work the same way in 2.0 and 2.1; what differs is the JavaScript
runtime underneath. The 2.1 runtime supports ECMAScript 2023. Moving a script is a matter of
changing its annotation to `@NApiVersion 2.1` and then dealing with the runtime differences. To
try 2.0 scripts on the new runtime before editing them, use the account-level execute-as-2.1
preferences.

## Which version a script runs as

Three places can determine the version a script runs as:

1. the `@NApiVersion` annotation in the script file,
2. the "Execute As Version" field on the script record,
3. the account-level preferences that run 2.0 scripts as 2.1.

> [!SOURCE-ERROR] The source indicates this script record field was once labelled with a shorter
> name that lacks the word "Version". Look for "Execute As Version" on current script records, and
> expect older material to refer to the same field by the shorter label.
<!-- source-error: topic:ss21_transition.from_ss20 | - | version_field_once_called_execute_as_without_version -->

How they interact:

- Validation always follows the annotation, never the preference. A file annotated `2.0` or `2.x`
  that contains 2.1-only syntax is rejected with a syntax error on upload. Either annotate it `2.1`
  or remove the 2.1-only syntax.
- Setting the "Execute As Version" field to 2.0 overrides the account preference for that script.
- The preference applies to eligible server scripts annotated 2.0. Client scripts are not affected
  by it.
- Script types that are unsupported or not recognized stay on 2.0 even with the preference on.

## Testing with the preferences

- The execute-as-2.1 preference exists for compatibility testing, not as a permanent setting.
- Investigate any failures in a sandbox.
- To stop, clear the preference under **Setup > Company > Preferences > General Preferences**.
  Scripts explicitly annotated `2.1` keep running as 2.1 after you clear it.

## What can break on the 2.1 runtime

Review these areas when moving a script to 2.1; details and examples are on the
[runtime behavior](ss21_transition.from_ss20.behaviors.md) and
[integration behavior](ss21_transition.from_ss20.integration.md) pages:

- identifiers that are now reserved words,
- strict mode,
- properties of error objects,
- parsing invalid JSON,
- reassigning a `const`,
- `for each...in` loops,
- the output format of `toLocaleDateString`,
- `toSource`,
- `parseInt` behaviour,
- promises in server scripts.

## RESTlets

What a RESTlet receives on POST, and what it returns, can differ under 2.1. To check a RESTlet:

1. Confirm which runtime it is actually running on.
2. Call every HTTP method it supports.
3. Compare the responses with those from 2.0.
4. Review what the calling integration assumes about the payloads.
5. Fix the script and test again.

## 2.1-only features

- The [N/llm](../modules/N_llm.md) and [N/pgp](../modules/N_pgp.md) modules require 2.1.
- [N/crypto/random](../modules/N_crypto_random.md) requires 2.1 in server scripts.
- 2.1 scripts can be debugged with the browser's developer tools.

## Example

Because this RESTlet is annotated `2.1`, it may use destructuring and the spread operator. With a
`2.0` or `2.x` annotation the same file would be rejected on upload, whatever the account
preference says.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/search'], (search) => {
  const get = ({ id }) => {
    const fields = search.lookupFields({ type: 'task', id, columns: ['title', 'priority'] });
    return JSON.stringify({ id, ...fields });
  };

  return { get };
});
```
