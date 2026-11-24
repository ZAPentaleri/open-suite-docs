# MapReduceScript
<!-- id: script:MapReduceScript -->

A map/reduce script is server-side code for jobs too large for a single pass that can still be cut
into many small, independent pieces. You supply the input, the work for one piece and, if needed,
the way pieces sharing a key are combined; NetSuite creates a job for every part and runs them in
parallel. It is the type to pick when the same short piece of logic has to be applied to a large
number of records, a little at a time.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `MapReduceScript` |
| Usage-unit limit | The source gives no per-script figure; limits apply to each invocation of a stage: `getInputData` 10,000, each `map` call 1,000, each `reduce` call 5,000, `summarize` 10,000 (see [Governance per invocation](#governance-per-invocation)) |
| Time limit | 3,600 seconds |
| Runs on | Server |
| SuiteScript 1.0 counterpart | None |
| Run modes | Manually or on a schedule |
| Parallelism | Configured on the deployment |
| Entry points | [`getInputData`](#getinputdatacontext) (required), [`map`](#mapcontext), [`reduce`](#reducecontext), [`summarize`](#summarizecontext) (optional) |

Related pages: [processing model](../topics/map_reduce.md), [yielding](../topics/map_reduce.yielding.md),
[governance](../topics/map_reduce.governance.md), [interruptions and retries](../topics/map_reduce.interruptions.md),
[deployment](../topics/map_reduce.deployment.md), [submission](../topics/map_reduce.submission.md),
[status page](../topics/map_reduce.status_page.md), [testing](../topics/map_reduce.testing.md),
[SDF object](../topics/sdf.mapreducescript.md), [N/task](../modules/N_task.md),
[N/search](../modules/N_search.md), [N/query](../modules/N_query.md), [N/file](../modules/N_file.md),
[N/dataset](../modules/N_dataset.md).

## How it runs

**Stages.** One execution passes through `getInputData`, `map`, a shuffle that the system runs on
its own to group pairs by key, `reduce`, and finally `summarize`. Every stage except the shuffle is
an entry point you can implement. `getInputData` is mandatory, at least one of `map` and `reduce`
must be present, and `summarize` is optional. The [processing model](../topics/map_reduce.md) page
covers the stages in depth.

**Division of labour.** The system splits the data into parts, creates a job per part and runs map
and reduce jobs in parallel. The degree of parallelism lives on the deployment, not in your code.
The script can be started manually or run on a schedule.

**Compared with a scheduled script.** When it nears its governance limits, a map/reduce script yields
and reschedules itself without any code from you; a [scheduled script](ScheduledScript.md) does not
(see [yielding](../topics/map_reduce.yielding.md)). Yielding does not take care of every limit,
though: some governance limits are outside its reach and still end work (see the next section).

**Good and poor fits.** It suits running the same brief logic over many records in small portions.
It is a poor fit when each item needs lengthy, complicated handling, such as loading and saving
several records per item.

**Typical jobs.** Transforming requisitions into purchase orders, applying a discount to invoices
that meet some criterion, working through duplicate customer records, emailing sales reps about the
tasks they still have open, and taking files from the File Cabinet, building new documents from
them and sending those documents to an external server.

**Data flow.** Pairs written in `map` go through the shuffle to `reduce` when the script has a reduce
stage; otherwise they go on to `summarize`. The elapsed-seconds figure for the input stage leaves
out idle time.

**Context members in brief.** The map context offers `isRestarted`, `executionNo`, `errors`, `key`,
`value` and the `write` method. The summary context offers `isRestarted`, `concurrency`,
`dateCreated`, `seconds`, `usage`, `yields`, `inputSummary`, `mapSummary`, `reduceSummary` and
`output`.

**Walking the error, key and output collections.** Several members (`errors`, `keys`, `output`) are
iterators, walked with `.iterator().each(callback)`. As with the general
[`Iterator`](../globals.md), the callback's return value drives the loop: `true` moves on to the
next item, `false` stops. The callback's parameter list is uncertain; see
[`mapContext.errors`](#mapcontexterrors).

**Managing scripts.** Map/reduce scripts can be managed with the SuiteCloud Development Framework,
copied to another account with Copy to Account (the control is at the upper right of the script
page), and reviewed with SuiteScript Analysis.

> [!SOURCE-ERROR] Where the source points to best practices for map/reduce scripts, the link it
> gives is about client scripts. Ignore that pointer and use the map/reduce pages listed above.
<!-- source-error: script:MapReduceScript | - | best_practices_pointer_says_client_scripts -->

### Problems in the source's samples

The source's own sample scripts contain mistakes. Do not copy them; the examples at the end of this
page are written independently.

> [!SOURCE-ERROR] In the source's invoice sample, the reduce stage applies every line of the record
> instead of working from the values handed to `reduce`. In your own scripts, base the reduce logic
> on `context.values`.
<!-- source-error: script:MapReduceScript | - | invoice_sample_reduce_applies_all_lines_not_reduce_values -->

> [!SOURCE-ERROR] The same invoice sample calls `getLineCount` with a plain string argument, whereas
> [`Record.getLineCount`](../modules/N_record.md) takes an options object with `sublistId`. Use the
> options form.
<!-- source-error: script:MapReduceScript | - | invoice_sample_getlinecount_positional_string_not_options -->

> [!SOURCE-ERROR] The source's letter-counting sample calls `test()` on a regular expression created
> with the global flag. Such a regular expression remembers `lastIndex` between calls, so repeated
> tests can miss matches. In plain JavaScript, drop the global flag or reset `lastIndex` before each
> test.
<!-- source-error: script:MapReduceScript | - | letter_count_sample_global_regexp_test_keeps_lastindex -->

### Governance per invocation

Limits are counted per function invocation, not across the whole execution, and there is no overall
duration limit for you to set. Scripts and workflows that your code triggers do not count toward
these limits. Full details are on the [governance](../topics/map_reduce.governance.md) page.

| Stage | Max usage units per invocation | Max time per invocation | Max instructions per invocation | When the usage limit is exceeded |
|---|---|---|---|---|
| `getInputData` | 10,000 | 60 minutes | 1,000,000,000 | The invocation ends and the job moves to `summarize` |
| `map` | 1,000 | 5 minutes | 100,000,000 | The invocation ends; jobs already processing normally finish, pending jobs are cancelled |
| `reduce` | 5,000 | 15 minutes | 100,000,000 | The invocation ends; jobs already processing normally finish, pending jobs are cancelled |
| `summarize` | 10,000 | 60 minutes | 1,000,000,000 | The script stops |

- There are hard limits, which interrupt an invocation, and soft limits, which cause a yield once
  the invocation is over.
- In map and reduce, a job that has used 10,000 units (a soft limit, checked after each invocation)
  yields, and a new job with the same priority and a later timestamp carries on. The deployment's
  yield-after-minutes field (3 to 60 minutes, default 60) causes yields too.
- The usage limit for `map` matches the mass update limit; the instruction limits for `map` and
  `reduce` match the user event limit, and those for `getInputData` and `summarize` match the
  scheduled script limit.
- Data persisted between stages is capped at 200 MB per execution (a hard limit). It counts pairs not
  yet mapped, pairs not yet reduced and pairs written as reduce results, but not pairs already
  processed or internal troubleshooting data. Exceeding it ends the invocation, leaves the stage and
  moves on to `summarize`; the limit is not reached during `summarize`. Returning a search with many
  columns as input inflates this figure. If you hit the cap, keep data in a custom field or a custom
  record instead.
- Errors named for these limits: `SSS_USAGE_LIMIT_EXCEEDED` (an invocation limit exceeded) and
  `PERSISTED_DATA_LIMIT_FOR_MAPREDUCE_SCRIPT_EXCEEDED` (the persisted-data cap exceeded).
- Keep each `map` and `reduce` invocation small.

### Retries and exit on error

What happens after an uncaught error in `map` or `reduce` is governed by two settings, `retryCount`
and `exitOnError`. Both go into a config block inside the object your script returns
(`return { getInputData, map, config: { ... } }`), not onto the deployment.

- `retryCount` (0 to 3) applies to `map` and `reduce` and says how many times an uncertain pair is
  retried; the count starts over for every new pair. A value of 2, for instance, allows three
  attempts per pair. After an uncaught error only the failing pair is retried; after a server restart
  all flagged unfinished pairs are. Without `retryCount`, a pair that failed with an uncaught error
  is not processed again, while pairs left uncertain by a server restart are all reprocessed.
- `exitOnError` (default `false`) concerns only uncaught errors in `map` or `reduce`; it has no effect
  on server restarts. When it is `true`, the stage is abandoned for `summarize` once the retries are
  used up.
- The order of evaluation is: retry the pair if retries remain, otherwise go to `summarize` if
  `exitOnError` is set, otherwise continue with the remaining pairs.

A third setting, the deployment's buffer size, controls how many pairs are flagged before progress is
saved; a value of 1 is recommended. See [deployment](../topics/map_reduce.deployment.md) and
[interruptions and retries](../topics/map_reduce.interruptions.md) for restart behaviour after server
disruptions.

## Entry points

### getInputData(context)
<!-- id: script:MapReduceScript.getInputData -->

Runs once per execution, at the very start (the input stage), and produces the data that the rest of
the script processes. This entry point is required.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.ObjectRef` | Object | - | |
| `context.isRestarted` | boolean | - | |

- `context.ObjectRef` (since 2015.2) is listed among the input context's members, although
  [`ObjectRef`](#objectref) is in fact a shape you return.
- `context.isRestarted` (since 2016.1) is read-only; see [`inputContext.isRestarted`](#inputcontextisrestarted).

> [!SOURCE-ERROR] The source lists `ObjectRef` as a member of the input context while also describing
> it as one of the forms a return value can take. Treat it as a return shape, not something to read
> from `context`.
<!-- source-error: script:MapReduceScript.getInputData | - | objectref_listed_as_inputcontext_member_but_is_return_form -->

The context object is [`inputContext`](#inputcontext).

**Returns:** `Array | Object | file.File | query.Query | dataset.Dataset | search.Search | inputContext.ObjectRef`.
The last form is an object reference (see [`ObjectRef`](#objectref)). The accepted forms are:

- an array or a plain object;
- a `file.File`, `query.Query`, `dataset.Dataset` or `search.Search` object;
- an object reference pointing at a file, a SuiteQL query, a saved query or a saved search:
  - SuiteQL: type `suiteql`, plus the `query` text and its `params`.
  - Saved query or saved search: type `query` or `search`, plus the `id`. There is no need to load
    the saved query or search yourself.
  - File: type `file`, plus the file's `id` or its absolute path.

How the input turns into key/value pairs:

- From a file, every line becomes one pair: the key is the line number starting at 0 and the value is
  the text of the line. The input file must not contain empty lines.
- From SuiteQL, the value reaching `map` is a string; run it through `JSON.parse` and read its
  `values` array to get the row's columns.
- Values that are not strings are turned into strings with `JSON.stringify` before they are stored,
  so the later stages always receive strings.

**Errors:** if `getInputData` throws, the job goes straight to `summarize` without a restart, and the
error is available in [`inputSummary.error`](#inputsummaryerror). This behaviour cannot be
configured.

**Governance:** at most 10,000 usage units, 60 minutes and 1,000,000,000 instructions per invocation;
exceeding the usage limit ends the invocation and moves the job to `summarize`.

If the input stage is run again after a disruption, it should return the same data as the first time.

> [!SOURCE-ERROR] The source's syntax sample for this entry point is malformed: parentheses are
> missing and there is a surplus closing brace. Do not copy it; follow the examples on this page.
<!-- source-error: script:MapReduceScript.getInputData | - | syntax_sample_missing_parens_extra_brace -->

### map(context)
<!-- id: script:MapReduceScript.map -->

Invoked once for every key/value pair that leaves the input stage. It emits new pairs with
`context.write`, which the shuffle then groups by key. `map` is required when the script has no
`reduce`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.errors` | iterator | - | |
| `context.executionNo` | number | - | |
| `context.key` | string | - | |
| `context.value` | string | - | |
| `context.isRestarted` | boolean | - | |

- `context.errors` and `context.executionNo` exist since 2018.1; `context.key` and `context.value`
  since 2015.2; `context.isRestarted` since 2016.1.
- What happens when a `map` invocation fails depends on `retryCount` (see
  [Retries and exit on error](#retries-and-exit-on-error)). Errors from earlier attempts on the
  current pair are in `context.errors`; every error of the stage is collected in
  [`mapSummary.errors`](#mapsummaryerrors).

The context object is [`mapContext`](#mapcontext).

**Returns:** `Void`

**Governance:** at most 1,000 usage units, 5 minutes and 100,000,000 instructions per invocation.
Exceeding the usage limit ends the invocation and cancels pending jobs.

### reduce(context)
<!-- id: script:MapReduceScript.reduce -->

Invoked once for every key together with the list of values grouped under it. With a `map` stage the
input comes from the shuffled map output; without one it comes straight from `getInputData`.
`reduce` is required when the script has no `map`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.errors` | iterator | - | |
| `context.executionNo` | number | - | |
| `context.key` | string | - | |
| `context.values` | string[] | - | |
| `context.isRestarted` | boolean | - | |

- `context.errors` and `context.executionNo` exist since 2018.1; `context.key` and `context.values`
  since 2015.2; `context.isRestarted` since 2016.1.
- Failure handling depends on `retryCount`. Errors from earlier attempts on this key are in
  `context.errors`; every error of the stage ends up in [`reduceSummary.errors`](#reducesummaryerrors).

The context object is [`reduceContext`](#reducecontext).

**Returns:** `Void`

**Governance:** at most 5,000 usage units, 15 minutes and 100,000,000 instructions per invocation.
Exceeding the usage limit ends the invocation and cancels pending jobs.

### summarize(context)
<!-- id: script:MapReduceScript.summarize -->

Runs once per execution, when the summarize stage is reached. Its context holds statistics for the
whole run (timings, usage, yields, concurrency), the per-stage summaries and the pairs written by
`reduce`. It is optional.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `context` | Object | Yes | |
| `context.dateCreated` | Date | - | |
| `context.seconds` | number | - | |
| `context.usage` | number | - | |
| `context.concurrency` | number | - | |
| `context.yields` | number | - | |
| `context.output` | iterator | - | |
| `context.inputSummary` | Object | - | |
| `context.reduceSummary` | Object | - | |
| `context.mapSummary` | Object | - | |
| `context.isRestarted` | boolean | - | |

Every member exists since 2015.2 except `context.isRestarted` (since 2016.1, read-only). Each is
described under [`summaryContext`](#summarycontext).

**Returns:** `Void`

**Governance:** at most 10,000 usage units, 60 minutes and 1,000,000,000 instructions per invocation;
exceeding the usage limit stops the script.

An error thrown in `summarize` stops the script, and this cannot be configured. A log entry written
here may show up twice, so duplicate summary log lines do not mean two runs. This stage is a good
place to log restarts (check `context.isRestarted`).

## Objects

### mapContext
<!-- id: script:MapReduceScript.mapContext -->

Passed to `map`. It holds the key/value pair to process, the `write` method that feeds the reduce
stage, and a few bookkeeping properties.

> [!SOURCE-ERROR] The source's description of this object is muddled: it describes `value` as though
> it were the key and calls `executionNo` a "type" property. Rely on the property sections below
> (`key` is the key, `value` is the value, `executionNo` counts attempts).
<!-- source-error: script:MapReduceScript.mapContext | - | value_described_as_key_and_executionno_type_property -->

#### mapContext.errors
<!-- id: script:MapReduceScript.mapContext#errors -->

Type `iterator`, since 2018.1. The serialized errors thrown by earlier attempts at the current pair.
It can contain `SSS_APP_SERVER_RESTART` when an attempt was cut short by an application server
disruption. Walk it with `context.errors.iterator().each(callback)`; according to this property's
description the callback receives `key` (string), `error` (string) and `executionNo` (number).
Return `true` from the callback to go on to the next error and `false` to stop.

> [!SOURCE-ERROR] The source's code sample for this property logs under a title that mentions a
> reduce error, although the code runs in the map context. Disregard the title; the property holds
> errors from earlier `map` attempts.
<!-- source-error: script:MapReduceScript.mapContext#errors | - | snippet_log_title_says_reduce_error_in_map_context -->

> [!SOURCE-ERROR] The three-argument callback described here conflicts with the general
> [`Iterator`](../globals.md), whose `each` callback takes a single argument (an object carrying the
> item in a `value` property). The source does not settle which is right; check what your callback
> actually receives before relying on either form. The same question applies to
> [`reduceContext.errors`](#reducecontexterrors), [`mapSummary.keys`](#mapsummarykeys) and
> [`summaryContext.output`](#summarycontextoutput).
<!-- source-error: script:MapReduceScript.mapContext#errors | - | each_callback_multi_param_vs_generic_iterator_one_param -->

#### mapContext.executionNo
<!-- id: script:MapReduceScript.mapContext#executionNo -->

Type `number`, since 2018.1. The attempt number for the current pair; use it to avoid doing the same
work twice. When `map` restarts for a pair, whatever the earlier attempt wrote is deleted. The source
gives no range for this property itself; the `executionNo` values reported in
[`mapSummary.errors`](#mapsummaryerrors) run from 1 to 4.

#### mapContext.key
<!-- id: script:MapReduceScript.mapContext#key -->

Type `string`, since 2015.2. The key of the pair being processed. Its origin depends on the input: the
element index for an array, the property name for an object, and the internal ID or the index for a
result set. Maximum length: 4,000 bytes.

> [!SOURCE-ERROR] Another page of the source (on map/reduce stages) gives the key limit as 3,000
> characters rather than 4,000 bytes; the governance material also says 3,000 characters and names
> the error `KEY_LENGTH_IS_OVER_3000_BYTES`. Which figure is right is not settled; keep keys well
> below both and carry bulky data in values.
<!-- source-error: script:MapReduceScript.mapContext#key | - | 3000_characters_on_map_reduce_stages_page -->

#### mapContext.value
<!-- id: script:MapReduceScript.mapContext#value -->

Type `string`, since 2015.2. The value of the pair being processed: the element for an array input,
the property value for an object input, or a `search.Result` serialized as a JSON string for a result
set. Maximum size: 1 MB.

> [!SOURCE-ERROR] The page on map/reduce stages states 10 MB for this limit instead of 1 MB (the
> governance material also says 10 MB, error `VALUE_LENGTH_IS_OVER_10_MB`). Which figure applies is
> not settled; design for the smaller one.
<!-- source-error: script:MapReduceScript.mapContext#value | - | 10_mb_on_map_reduce_stages_page -->

#### mapContext.isRestarted
<!-- id: script:MapReduceScript.mapContext#isRestarted -->

Type `boolean`, since 2016.1. According to this property's description, `true` means `map` has already
run for this pair before (an earlier attempt was interrupted), and in that case anything the earlier
attempt wrote for the pair has been deleted. When it is `true`, guard against processing the pair a
second time. See also [`executionNo`](#mapcontextexecutionno) and [`errors`](#mapcontexterrors).

> [!SOURCE-ERROR] The interruption material contradicts that reading: a restarted job reprocesses its
> whole unfinished batch, so `true` does not by itself prove that this particular pair ran before. The
> source does not reconcile the two; treat `true` as "this pair may have been processed" and make
> changes outside the map/reduce framework (records you create, for example) idempotent, because
> those are never rolled back.
<!-- source-error: script:MapReduceScript.mapContext#isRestarted | - | isrestarted_true_means_ran_before_vs_restarted_batch_only -->

#### mapContext.write(options)
<!-- id: script:MapReduceScript.mapContext#write -->

Emits a key/value pair; the pairs go to the shuffle and then to `reduce`. When the script has both
`map` and `reduce`, you must call it, or `reduce` never runs.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.key` | String \| object | Yes | |
| `options.value` | String \| object | Yes | |

- `options.key`: an object is passed through `JSON.stringify` first.
- `options.value`: an object is passed through `JSON.stringify` first.

**Returns:** `Void`

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2015.2

### reduceContext
<!-- id: script:MapReduceScript.reduceContext -->

Passed to `reduce`. It holds one key and all of its values, the `write` method that feeds the
summarize stage, and bookkeeping properties.

> [!SOURCE-ERROR] The source describes `values` as "the key being processed", which is a copying
> error: `values` is the list of values for the key, as described below.
<!-- source-error: script:MapReduceScript.reduceContext | - | values_described_as_key_being_processed -->

#### reduceContext.errors
<!-- id: script:MapReduceScript.reduceContext#errors -->

Type `iterator`, since 2018.1. Errors from earlier attempts at the current key; may include
`SSS_APP_SERVER_RESTART`. Iterate with `context.errors.iterator().each(callback)`; the callback is
described as receiving `key` (string), `error` (string) and `executionNo` (number), and returns `true`
to continue or `false` to stop. The callback form is uncertain; see
[`mapContext.errors`](#mapcontexterrors).

#### reduceContext.executionNo
<!-- id: script:MapReduceScript.reduceContext#executionNo -->

Type `number`, since 2018.1. The attempt number for the current key, from 1 to 4. Use it to avoid
duplicate processing. When `reduce` restarts for a key, the earlier attempt's writes are deleted.

#### reduceContext.key
<!-- id: script:MapReduceScript.reduceContext#key -->

Type `string`, since 2015.2. The key being reduced. With a map stage it is a key that `map` wrote;
without one it is the array index, the object property name, or the internal ID of a result-set row.
Maximum length: 4,000 bytes.

> [!SOURCE-ERROR] The 4,000-byte figure conflicts with the governance material, which limits keys to
> 3,000 characters. The source does not reconcile them; staying below 3,000 characters is the safe
> choice.
<!-- source-error: script:MapReduceScript.reduceContext#key | - | key_4000_bytes_vs_governance_3000_chars -->

#### reduceContext.values
<!-- id: script:MapReduceScript.reduceContext#values -->

Type `string[]`, since 2015.2. Every value grouped under the current key, in lexicographical order.
With a map stage these are the values `map` wrote; without one they are array elements, object
property values, or `search.Result` objects serialized as JSON strings. Each single value can be at
most 1 MB.

> [!SOURCE-ERROR] The 1 MB per-value limit conflicts with the 10 MB value limit in the governance
> material. The source does not say which applies; plan for 1 MB.
<!-- source-error: script:MapReduceScript.reduceContext#values | - | value_1_mb_vs_governance_10_mb -->

#### reduceContext.isRestarted
<!-- id: script:MapReduceScript.reduceContext#isRestarted -->

Type `boolean`, since 2016.1. According to this property's description, `true` means `reduce` has
already run for this key and was interrupted; the earlier attempt's writes for the key have been
deleted (after a reduce restart only the most recent writes per pair are kept). When it is `true`,
avoid repeating work that already took effect. See also [`executionNo`](#reducecontextexecutionno)
and [`errors`](#reducecontexterrors).

> [!SOURCE-ERROR] The source repeats a sentence in this property's description. The meaning given
> above does not depend on the repeated text.
<!-- source-error: script:MapReduceScript.reduceContext#isRestarted | - | sentence_repeated_in_description -->

> [!SOURCE-ERROR] As for [`mapContext.isRestarted`](#mapcontextisrestarted), the interruption material
> says a restarted job reprocesses its whole unfinished batch, so `true` does not prove that this key
> was processed before. The source does not settle this; treat `true` as a hint and keep external
> changes idempotent.
<!-- source-error: script:MapReduceScript.reduceContext#isRestarted | - | isrestarted_true_means_ran_before_vs_restarted_batch_only -->

#### reduceContext.write(options)
<!-- id: script:MapReduceScript.reduceContext#write -->

Emits a key/value pair that is handed to the summarize stage, where it appears in
[`summaryContext.output`](#summarycontextoutput).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.key` | String \| object | Yes | |
| `options.value` | String \| object | Yes | |

- `options.key`: an object is passed through `JSON.stringify` first.
- `options.value`: an object is passed through `JSON.stringify` first.

**Returns:** `Void`

**Governance:** not documented; assumed to be free (0 units) by project convention.

**Since:** 2015.2

### ObjectRef
<!-- id: script:MapReduceScript.ObjectRef -->

A plain object that `getInputData` can return to point at the input data instead of returning the
data itself, for example a SuiteQL query or a reference to a saved search. The return type of
`getInputData` names it `inputContext.ObjectRef`.

#### ObjectRef.id
<!-- id: script:MapReduceScript.ObjectRef#id -->

Type `string | number`, since 2015.2. The internal ID or script ID of the object referred to, such as
the ID of a saved search.

#### ObjectRef.type
<!-- id: script:MapReduceScript.ObjectRef#type -->

Type `string`, since 2015.2. The source names `search` as the only allowed value.

> [!SOURCE-ERROR] Allowing only `search` contradicts the source's own reference forms and examples,
> which also use `query`, `file` and `suiteql` (see [`getInputData`](#getinputdatacontext)). Which is
> right is not settled; verify any type other than `search` in your account.
<!-- source-error: script:MapReduceScript.ObjectRef#type | - | values_search_only_vs_query_file_suiteql_examples -->

### summaryContext
<!-- id: script:MapReduceScript.summaryContext -->

Passed to `summarize`. It carries statistics for the whole execution, plus
[`inputSummary`](#inputsummary), [`mapSummary`](#mapsummary), [`reduceSummary`](#reducesummary) and
[`output`](#summarycontextoutput).

#### summaryContext.dateCreated
<!-- id: script:MapReduceScript.summaryContext#dateCreated -->

Type `Date`, since 2015.2. When the script started.

#### summaryContext.seconds
<!-- id: script:MapReduceScript.summaryContext#seconds -->

Type `number`, since 2015.2. Total elapsed seconds for the run.

#### summaryContext.usage
<!-- id: script:MapReduceScript.summaryContext#usage -->

Type `number`, since 2015.2. Total usage units consumed.

#### summaryContext.concurrency
<!-- id: script:MapReduceScript.summaryContext#concurrency -->

Type `number`, since 2015.2. The largest number of tasks that ran in parallel. It can fall below the
concurrency set on the deployment, because tasks that were waiting (pending) are not counted.

#### summaryContext.yields
<!-- id: script:MapReduceScript.summaryContext#yields -->

Type `number`, since 2015.2. How many times the script yielded in total.

#### summaryContext.output
<!-- id: script:MapReduceScript.summaryContext#output -->

Type `iterator`, since 2015.2. The pairs written with [`reduceContext.write`](#reducecontextwriteoptions),
ordered lexicographically by key. Iterate with `context.output.iterator().each(callback)`; the
callback is described as receiving `key` and `value`, both strings, and returns `true` to continue or
`false` to stop (the callback form is uncertain; see [`mapContext.errors`](#mapcontexterrors)).

#### summaryContext.isRestarted
<!-- id: script:MapReduceScript.summaryContext#isRestarted -->

Type `boolean`, read-only, since 2016.1. `true` means `summarize` itself was restarted, which happens
after a JVM restart. The summary data of the incomplete run is deleted.

### inputSummary
<!-- id: script:MapReduceScript.inputSummary -->

Statistics for the input stage, read as `summaryContext.inputSummary`. Unlike the map and reduce
summaries, no yields count and no concurrency figure are documented for the input stage.

#### inputSummary.dateCreated
<!-- id: script:MapReduceScript.inputSummary#dateCreated -->

Type `Date`, since 2015.2. When `getInputData` started.

#### inputSummary.usage
<!-- id: script:MapReduceScript.inputSummary#usage -->

Type `number`, since 2015.2. Usage units consumed by `getInputData`.

#### inputSummary.error
<!-- id: script:MapReduceScript.inputSummary#error -->

Type `string`, since 2015.2. The serialized error thrown by `getInputData`. It is set only when
`getInputData` threw, so test for its presence.

#### inputSummary.seconds
<!-- id: script:MapReduceScript.inputSummary#seconds -->

Type `number`, since 2015.2. Seconds spent in `getInputData`, excluding idle time.

### mapSummary
<!-- id: script:MapReduceScript.mapSummary -->

Statistics for the map stage, read as `summaryContext.mapSummary`.

#### mapSummary.dateCreated
<!-- id: script:MapReduceScript.mapSummary#dateCreated -->

Type `Date`, since 2015.2. When the first `map` invocation started.

#### mapSummary.usage
<!-- id: script:MapReduceScript.mapSummary#usage -->

Type `number`, since 2015.2. Total usage units for the map stage.

#### mapSummary.seconds
<!-- id: script:MapReduceScript.mapSummary#seconds -->

Type `number`, since 2015.2. Total seconds spent in the map stage.

#### mapSummary.yields
<!-- id: script:MapReduceScript.mapSummary#yields -->

Type `number`, since 2015.2. Total yields during the map stage.

#### mapSummary.keys
<!-- id: script:MapReduceScript.mapSummary#keys -->

Type `iterator`, since 2015.2. The keys that `getInputData` handed to `map`. They are sorted
numerically when every key is a number, and lexicographically as soon as any key is a string. The
callback is described as receiving `key` (string), `executionCount` (number) and `completionState`
(string: `COMPLETE`, `FAILED` or `PENDING`), and returns `true` to continue or `false` to stop (the
callback form is uncertain; see [`mapContext.errors`](#mapcontexterrors)). `PENDING` means the stage
was left before the key was processed, for example because `exitOnError` is `true`.

#### mapSummary.errors
<!-- id: script:MapReduceScript.mapSummary#errors -->

Type `iterator`, since 2015.2. Every serialized error raised in `map`, which can include
`SSS_APP_SERVER_RESTART`. The callback is described as receiving `key` (string), `error` (string) and
`executionNo` (number, from 1 to 4), and returns `true` to continue or `false` to stop.

#### mapSummary.concurrency
<!-- id: script:MapReduceScript.mapSummary#concurrency -->

Type `number`, since 2015.2. The largest number of map tasks that ran in parallel. Pending tasks are
not counted, so it can be below the deployment's concurrency setting.

### reduceSummary
<!-- id: script:MapReduceScript.reduceSummary -->

Statistics for the reduce stage, read as `summaryContext.reduceSummary`.

#### reduceSummary.concurrency
<!-- id: script:MapReduceScript.reduceSummary#concurrency -->

Type `number`, since 2015.2. The largest number of reduce tasks that ran in parallel; pending tasks
are not counted, so it can be lower than the deployment setting.

#### reduceSummary.seconds
<!-- id: script:MapReduceScript.reduceSummary#seconds -->

Type `number`, since 2015.2. Total seconds spent in the reduce stage.

#### reduceSummary.keys
<!-- id: script:MapReduceScript.reduceSummary#keys -->

Type `iterator`, since 2015.2. The keys passed to `reduce`, whether they came from `map` or from
`getInputData`, in lexicographical order. The callback is described as receiving `key` (string),
`executionCount` (number) and `completionState` (string: `COMPLETE`, `FAILED` or `PENDING`), and
returns `true` to continue or `false` to stop. `PENDING` means the stage ended before the key was
processed, for example when `exitOnError` is `true`.

#### reduceSummary.dateCreated
<!-- id: script:MapReduceScript.reduceSummary#dateCreated -->

Type `Date`, since 2015.2. When the reduce stage started.

#### reduceSummary.usage
<!-- id: script:MapReduceScript.reduceSummary#usage -->

Type `number`, since 2015.2. Total usage units for the reduce stage.

#### reduceSummary.yields
<!-- id: script:MapReduceScript.reduceSummary#yields -->

Type `number`, since 2015.2. Total yields during the reduce stage.

#### reduceSummary.errors
<!-- id: script:MapReduceScript.reduceSummary#errors -->

Type `iterator`, since 2015.2. Every serialized error raised in `reduce`, possibly including
`SSS_APP_SERVER_RESTART`. The callback is described as receiving `key` (string), `error` (string) and
`executionNo` (number, from 1 to 4), and returns `true` to continue or `false` to stop.

### inputContext
<!-- id: script:MapReduceScript.inputContext -->

Passed to `getInputData`. Its members are [`isRestarted`](#inputcontextisrestarted) and, as the
source lists it, [`ObjectRef`](#objectref).

#### inputContext.isRestarted
<!-- id: script:MapReduceScript.inputContext#isRestarted -->

Type `boolean`, read-only, since 2016.1. `getInputData` normally runs once; `true` means it is running
again after a server restart. Checking it helps you write a sturdier input stage, for example one
that does not repeat side effects.

## Examples

A supplier drops a cost feed into the File Cabinet: one line per item, `internalId,unitCost`, with a
header line and no blank lines. The script reads the file by reference, keys each line by item so
duplicate lines meet in `reduce`, refuses to apply conflicting costs, and reports in `summarize`. The
config block gives each pair one retry and keeps going after failures. The file ID comes from a
script parameter. The iterator callbacks use the multi-argument form described for these members;
verify it in your account (see [`mapContext.errors`](#mapcontexterrors)).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/runtime', 'N/record', 'N/error', 'N/log'], (runtime, record, error, log) => {
  const getInputData = () => {
    const fileId = runtime.getCurrentScript().getParameter({ name: 'custscript_cost_feed_file' });
    // File input: key = line number (from 0), value = line text
    return { type: 'file', id: fileId };
  };

  const map = (context) => {
    if (context.key === '0') {
      return; // header line
    }
    const [itemId, rawCost] = context.value.split(',').map((part) => part.trim());
    const cost = Number(rawCost);
    if (!itemId || Number.isNaN(cost) || cost < 0) {
      log.audit({ title: 'Skipped feed line', details: `line ${context.key}: ${context.value}` });
      return;
    }
    // Earlier writes for this pair are discarded on restart, so writing again is safe.
    context.write({ key: itemId, value: cost.toFixed(2) });
  };

  const reduce = (context) => {
    const distinct = [...new Set(context.values)];
    if (distinct.length > 1) {
      throw error.create({
        name: 'CONFLICTING_COST_LINES',
        message: `Item ${context.key} appears with costs ${distinct.join(', ')}`
      });
    }
    record.submitFields({
      type: record.Type.INVENTORY_ITEM,
      id: context.key,
      values: { cost: distinct[0] }
    });
    context.write({ key: context.key, value: distinct[0] });
  };

  const summarize = (summary) => {
    if (summary.inputSummary.error) {
      log.error({ title: 'Cost feed could not be read', details: summary.inputSummary.error });
      return;
    }

    let reported = 0;
    summary.reduceSummary.errors.iterator().each((key, err, executionNo) => {
      log.error({ title: `Item ${key} not updated (attempt ${executionNo})`, details: err });
      reported += 1;
      return reported < 50; // false stops the loop after 50 entries
    });

    let updated = 0;
    summary.output.iterator().each(() => {
      updated += 1;
      return true;
    });

    log.audit({
      title: 'Cost feed finished',
      details: `${updated} items updated; ${summary.usage} units, ${summary.seconds}s, ${summary.yields} yields`
    });
  };

  return {
    getInputData,
    map,
    reduce,
    summarize,
    config: { retryCount: 1, exitOnError: false }
  };
});
```

A map-only script fed by a SuiteQL reference. Each row reaches `map` as a JSON string; parse it and
read its `values` array to get the selected columns. With no `reduce`, `map` is required and nothing
has to be written. The query text is illustrative: SuiteQL table and column names come from your
account's Records Catalog (see [SuiteQL](../topics/suiteql.md)).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType MapReduceScript
 */
define(['N/record', 'N/log'], (record, log) => {
  const getInputData = () => ({
    type: 'suiteql',
    query: 'SELECT id, priority FROM supportcase WHERE status = ?',
    params: ['2']
  });

  const map = (context) => {
    if (context.isRestarted) {
      log.audit({ title: 'Map retried', details: `pair ${context.key}, attempt ${context.executionNo}` });
    }
    const [caseId, priority] = JSON.parse(context.value).values;
    if (String(priority) !== '1') {
      record.submitFields({ type: record.Type.SUPPORT_CASE, id: caseId, values: { priority: '1' } });
    }
  };

  const summarize = (summary) => {
    summary.mapSummary.keys.iterator().each((key, executionCount, completionState) => {
      if (completionState !== 'COMPLETE') {
        log.error({ title: `Pair ${key} ended ${completionState}`, details: `${executionCount} attempts` });
      }
      return true;
    });
  };

  return { getInputData, map, summarize };
});
```
