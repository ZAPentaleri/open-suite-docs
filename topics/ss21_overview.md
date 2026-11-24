# SuiteScript 2.1 overview
<!-- id: topic:ss21_overview -->

Sub-topics:

- [SuiteScript 2.1 advantages](ss21_advantages.md)
- [SuiteScript 2.1 script basics](ss21_basics.md)
- [SuiteScript 2.1 Hello World tutorial](ss21_hello_world.md)
- [SuiteScript 2.1 custom modules](custom_modules.md)
- [SuiteScript 2.1 custom pages](custom_pages.md)
- [SuiteScript 2.1 JSDoc validation](jsdoc_validation.md)
- [Scripting records and subrecords](records_and_subrecords.md)
- [Record actions and macros](record_actions_macros.md)

See also: [Global Objects](global_objects.md), [SuiteScript 2.1 Modules](suitescript_modules.md),
[Transitioning to SuiteScript 2.1](ss21_transition.md).

SuiteScript 2.1 is the current version of NetSuite's JavaScript API. Its pages cover the basics of
writing a script, how a script file is laid out, how a script goes from file to deployment, why
2.1 is preferred, the terms used across the reference, where to find help, reserved words, and how
versions are declared.

## The runtime

- Server scripts run on the Graal engine, which supports ECMAScript 2023. Modern syntax such as the
  spread operator is available.
- Client scripts run in the user's browser and get whatever ECMAScript version that browser
  supports.
- 2.1 code can be debugged with the browser's own debugger.
- Moving to 2.1 may make scripts run faster.

## Version deadline

Scripts written for 1.0 or for 2.x must be updated to 2.1 by release 2028.2. The schedule and the
migration options are in [Transitioning to SuiteScript 2.1](ss21_transition.md).

## Client scripts: cautions

- A 2.1 client script deployed to all records also runs on subrecords, which can break pages that
  use them. If a client script must be deployed to all records, write it in ECMAScript 5.1 syntax.
- Client scripts are not supported in the scriptable cart.

## Global objects

Every 2.1 script can use these without importing anything: `define`, `require`, `log`, `util`,
`toString`, `JSON`, `Promise` and `Iterator`. They are described in
[Global Objects](global_objects.md); the importable modules are listed in
[SuiteScript 2.1 Modules](suitescript_modules.md).

## AI modules

Three modules call Oracle Cloud Infrastructure (OCI) AI services:

| Module | Backed by | Use |
|---|---|---|
| [N/llm](../modules/N_llm.md) | OCI Generative AI | Generating and evaluating text with a large language model |
| [N/documentCapture](../modules/N_documentCapture.md) | OCI Document Understanding | Extracting data from documents such as invoices, receipts and contracts |
| [N/machineTranslation](../modules/N_machineTranslation.md) | OCI Language | Translating text |

How an N/llm request travels: the script calls the module, NetSuite forwards the request to OCI
Generative AI, the model produces a response, and NetSuite hands it back to the script.

- The default model is Cohere Command A.
- When a prompt is evaluated from Prompt Studio, the model family is the one Prompt Studio sets.
- Data sent to these services is not used to train the models.
- The features are only available in certain regions, and data may be processed in other regions.
- Always validate AI output before acting on it; NetSuite takes no responsibility for content the
  models generate.

## Example

A 2.1 user event script that uses destructuring, arrow functions, `Object.fromEntries` and the
spread operator, all available on the server runtime.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/log'], (log) => {
  const WATCHED_FIELDS = ['title', 'priority', 'duedate'];

  const afterSubmit = ({ newRecord, type }) => {
    const values = Object.fromEntries(
      WATCHED_FIELDS.map((fieldId) => [fieldId, newRecord.getValue({ fieldId })])
    );
    const entry = { ...values, taskId: newRecord.id, event: type };
    log.audit({ title: 'Task saved', details: JSON.stringify(entry) });
  };

  return { afterSubmit };
});
```
