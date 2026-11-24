# SuiteScript Best Practices
<!-- id: topic:suitescript_best_practices -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [N/runtime](../modules/N_runtime.md), [N/search](../modules/N_search.md),
[N/record](../modules/N_record.md), [N/url](../modules/N_url.md),
[SuiteScript Governance and Limits](governance.md).

Practical guidance, grouped the way the source groups it: general coding, client scripts,
Map/Reduce, scheduled scripts, Suitelets and custom UI, user event scripts, performance, and searches.

## General habits

- Try a script against test data before it is allowed near live data, and test each component on
  its own.
- Write record, field, sublist and tab IDs in lower case.
- Start the IDs you choose for custom scripts and deployments with an underscore.
- Never hard-code passwords.
- When code is shared by several forms, retest it on every one of them.
- Assume that data can be missing or inconsistent, and handle that case.
- Keep reusable functions in a common library file, put your code in its own namespace, and give
  function names a unique prefix so they cannot collide with other code.
- Use library functions for dates, currency and XML rather than hand-written parsing.
- Name functions after the events they handle.
- Refer to things by their static IDs, never by their display names.
- Comment the code thoroughly.

Custom code has no access to the DOM of NetSuite's own pages; build UI with the SuiteScript APIs for
UI components instead. The `password` and `password2` fields are supported for scripting.

### Code style

- Keep functions small and readable and avoid deep nesting.
- Give functions, variables and IDs meaningful names; avoid one-letter variable names.
- Write function names in lowerCamelCase and globals and constants in UPPER_SNAKE_CASE.
- Variable names can carry a prefix that tells their type, for example `st_title` (string),
  `int_total_count` (integer), `fl_price` (float), `b_is_done` (boolean), `arr_phone_calls` (array),
  `rec_customer` (record) and `dt_first_billing_date` (date).
- Handle errors with `try` / `catch` / `finally`.
- Resolve URLs with [N/url](../modules/N_url.md) instead of hard-coding them.
- Prefer script parameters over hard-coded values. Read them through
  [`runtime.getCurrentScript()`](../modules/N_runtime.md) and the returned script object's
  [`getParameter`](../modules/N_runtime.md) method.
- Pass arguments by name (options objects) rather than by position.
- Use the enums the modules provide instead of literal strings.
- Do not use reserved words as names.
- The modules listed in `define` must appear in the same order as the parameters of the callback
  that receives them.

> [!SOURCE-ERROR] The variable-prefix list in the source writes the object prefix with a capital
> letter (`Obj`) while every other prefix is lower case. Treat it as a lower-case prefix like the rest.
<!-- source-error: topic:suitescript_best_practices | - | object_var_prefix_written_Obj_others_lowercase -->

> [!SOURCE-ERROR] The source's sample for reading a script parameter passes positional arguments in
> the SuiteScript 1.0 style, and its braces do not balance, so it would not run as written. In 2.x,
> call `getParameter` with an options object, as in the example at the end of this page.
<!-- source-error: topic:suitescript_best_practices | - | getparameter_sample_uses_positional_ss1_style_args -->
<!-- source-error: topic:suitescript_best_practices | - | getparameter_sample_has_unbalanced_braces -->

> **Watch out:** inline comments placed inside an object literal are not allowed. In a SuiteScript 2.1
> script such a comment can break the script without any error being reported.

### File names

Build a script file name from the company, a script-type code and the requirement, ending in `.js`;
for example `MyCompany_CS_SetTaxable.js`. The script-type codes are:

| Code | Code | Code |
|---|---|---|
| `CS` | `UE` | `SL` |
| `RL` | `PL` | `SC` |
| `MR` | `Gl` [sic] | `WA` |
| `MU` | `BI` | |

> [!SOURCE-ERROR] The SuiteGL code is written `Gl` [sic], with a lower-case second letter, although
> all the other codes are upper case. It is shown here as the source has it; an upper-case form is the
> likelier intent, but the source does not say so.
<!-- source-error: topic:suitescript_best_practices | - | suitegl_file_code_written_Gl_others_uppercase -->

### Logging and deployment status

- While testing, deploy with the Testing status, or with Released and a test audience.
- Use the Debug log level while testing and Audit in production; once the script is released, Error
  or Emergency keeps the log short.
- At important steps, log the record type and ID at the Audit level.
- Your custom scripts are listed under Customization > Scripting > Scripts.

### Data caveats

- Server scripts run in the account's time zone.
- Multibyte characters take up more of a field's capacity: a transaction number field that holds 45
  English characters holds fewer Chinese ones.
- Integers beyond JavaScript's safe-integer range must be handled as `BigInt` or kept as quoted
  strings.

## Time limits

A single server execution that runs longer than the time limit for its script type fails with
`SSS_TIME_LIMIT_EXCEEDED`. The limit depends on the script type or the application, and it also
applies to scripts started by other scripts.

Common reasons for long executions:

- many record operations;
- many transactions on the same records;
- many user events or workflows set off along the way;
- slow searches or updates.

Aim for about 5 seconds per execution in general. By script type the targets are about 5 seconds for
user event scripts, 10 seconds for Suitelets and portlets, and 5 minutes for scheduled scripts.

## Script-wide recommendations

- Write every script in SuiteScript 2.1.
- Combine scripts that do similar work, and inactivate deployments that are no longer used.
- Move heavy user event work to asynchronous processing.
- Prefer running as a role with the right permissions over granting permissions in code, and run as
  Administrator only when it is really needed.
- In production, use the Error or Emergency log level.
- Apply execution-context filtering to client and user event scripts.
- In `afterSubmit`, avoid saving the same record more than once; in `beforeSubmit`, avoid loading and
  submitting records.
- For creating or editing records, use dynamic mode; for reading, use standard mode.
- When you need only a few fields, call [`search.lookupFields`](../modules/N_search.md) rather than
  loading the record.
- Pick a trigger that saves you from reloading the record.
- Keep the number of load, search and save calls low.
- To update many child records at once, use child records that support inline editing.
- The Application Performance Management (APM) SuiteApp can measure how scripts perform.

## Client scripts

- Prefer record-level client scripts over form-level ones.
- [`Record.setValue`](../modules/N_record.md) and
  [`Record.setCurrentSublistValue`](../modules/N_record.md) start field sourcing asynchronously. To
  work with the sourced values synchronously, use the `postSourcing` entry point or set
  `forceSyncSourcing` to `true`.
- With Advanced Employee Permissions, which fields a user can see changes, and it can change again
  for each employee record being viewed. Check that a field exists (with `getFields`) before you use
  it. A field the user has no access to reads as `null`; an empty field the user can access reads as
  an empty string.
- A nested script should not select a different line of the sublist that is currently being edited.
- `setValue` takes a raw value of the field's type, whereas `setText` takes the text as the user sees
  it, formatted by the user's preferences. So the same value may need different text for different
  users: a number such as two thousand and thirty-nine hundredths can be written with different
  thousands and decimal separators, and a date can be written as 6/9/2026 or as 9/6/2026 depending on
  the date format.

> [!SOURCE-ERROR] In the source's `setText` number example, the second way of writing the amount has
> one zero too few, so the two forms do not show the same number. Do not copy that string as a format
> sample; generate the text from the user's actual number format.
<!-- source-error: topic:suitescript_best_practices | - | settext_number_example_2_00_39_missing_a_zero -->

- While testing form scripts, use a hard refresh (Ctrl+Refresh) to clear the browser cache.
- To debug a client script, you can put a `debugger` statement on its first line.
- Avoid accessing sensitive fields.
- Client scripts in bundles must not be hidden. When a bundled client script uses a library, upload
  the library unhidden, set its installation preference to unhidden, and link the library to the
  client script.

## Map/Reduce scripts

Usage-unit limit per invocation of each stage:

| Stage | Limit (usage units) |
|---|---|
| `getInputData` | 10,000 |
| `map` | 1,000 |
| `reduce` | 5,000 |
| `summarize` | 10,000 |

- From `getInputData`, return a search (or a reference to a saved search, for example an object that
  gives the type `search` and an ID such as 1234), not an array of results. Returning a result array
  can time out, while a returned search object gets a longer time limit.
- Keep `map` and `reduce` light, handling one record per invocation.
- If the work is complex and touches many records in one function, use a scheduled script instead.
- Check `isRestarted` in every entry point.
- The buffer size defaults to 1. Leave it at 1 when processing records: a larger buffer means more
  records are processed twice after a restart.
- Some governance usage in Map/Reduce is not yielded automatically.

## Scheduled and queued processing

- Scheduled and not-scheduled scripts share the same pool of processors.
- For many lightweight records, prefer Map/Reduce over a scheduled script.
- Without SuiteCloud Plus, all queue deployments share a single queue.
- Create as many not-scheduled deployments as the number of calls you expect at the same time.
- Too many pending scripts build up a backlog.
- SuiteScript 2.1 scheduled scripts have no recovery points and cannot yield.

To run a script every hour around the clock, set up the deployment like this: Deployed checked, a
Daily event repeating every 1 day, start time 12 am, repeat every hour, no end date, status Scheduled,
log level Error, executing as Administrator. If the start time is anything other than midnight, the
hourly runs stop at midnight.

Schedule heavy jobs between 2 am and 6 am Pacific time; runs between 6 am and 6 pm Pacific may be
slower.

## Suitelets and custom UI

Suitelets suit three jobs: generating pages, returning XML or text, and redirecting requests.

- Keep UI objects small: sublists under 100 rows, on-demand select options under 100, lists under
  200 rows.
- Consider an inline HTML field before building a fully custom page.
- Load external content in inline HTML with Subresource Integrity.
- Escape parameters you put into external URLs, and resolve URLs instead of hard-coding them. For a
  Suitelet's URL, combine `url.resolveDomain` with `url.resolveScript` (see
  [N/url](../modules/N_url.md)).
- To display a page inside an iframe, the URL parameter `ifrmcntnr=T` is used.
- For credentials that users enter in custom UI, use [N/auth](../modules/N_auth.md) and
  [N/crypto](../modules/N_crypto.md).
- Avoid Available Without Login unless you need it. Such Suitelets have no user context and can expose
  employee data.
- With Advanced Employee Permissions, a Suitelet that runs as Administrator can expose employee data.

## User event scripts

- Back up critical logic with a scheduled clean-up job.
- Share logic through a common module rather than chaining user events.
- Use the event type and the execution context to limit when the script does its work.
- Do work that depends on the database in `afterSubmit`.
- To carry a value from `beforeLoad` to `afterSubmit`, store it in a hidden custom field.
- In `beforeSubmit`, line totals must agree with the summary totals.
- Many `beforeLoad` scripts, bundled ones included, slow down loading the record.
- Web store activity triggers scripts on sales orders, cases and customers.
- Use execution-context filtering.

## Searches

- Filter out inactive records, use short date ranges, and request fewer columns.
- Prefer the operators starts with, between and within over contains or formulas.
- Merge searches on the same record type into one.

## Example

A user event script that acts only on newly created support cases. It reads the priority to watch
from a script parameter instead of hard-coding it, looks up one field of the case's company instead of
loading that record, and logs the record type and ID at the Audit level.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/runtime', 'N/search', 'N/log'], (runtime, search, log) => {
  const PARAM_PRIORITY = 'custscript_case_watch_priority';

  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    const caseRec = context.newRecord;
    try {
      const watchPriority = runtime.getCurrentScript().getParameter({ name: PARAM_PRIORITY });
      if (String(caseRec.getValue({ fieldId: 'priority' })) !== String(watchPriority)) {
        return;
      }
      const companyId = caseRec.getValue({ fieldId: 'company' });
      const found = search.lookupFields({
        type: search.Type.ENTITY,
        id: companyId,
        columns: ['email']
      });
      log.audit({
        title: `${caseRec.type} ${caseRec.id}`,
        details: `Watched priority; company contact ${found.email || 'none'}`
      });
    } catch (e) {
      log.error({ title: `Case ${caseRec.id} check failed`, details: e.message });
    } finally {
      log.debug({ title: 'Case watch', details: `Finished ${caseRec.id}` });
    }
  };

  return { afterSubmit };
});
```
