# Map/reduce governance
<!-- id: topic:map_reduce.governance -->

Part of: [Map/reduce processing model](map_reduce.md)

See also: [MapReduceScript](../script-types/MapReduceScript.md), [Map/reduce yielding](map_reduce.yielding.md),
[Map/reduce interruption handling](map_reduce.interruptions.md).

A map/reduce script's limits apply **to each function invocation**, not to the execution as a whole.
Every call of [`getInputData`](../script-types/MapReduceScript.md),
[`map`](../script-types/MapReduceScript.md), [`reduce`](../script-types/MapReduceScript.md) and
[`summarize`](../script-types/MapReduceScript.md) gets its own budget. The execution as a whole has
no duration limit, and you cannot impose one.

The limits fall into two groups:

- **Hard limits** cut off the invocation that reaches them.
- **Soft limits** let the invocation finish and then make the job yield.

## Hard limits per invocation

| Stage | Usage units | Time | Instructions | When the usage limit is exceeded |
|---|---|---|---|---|
| getInputData | 10,000 | 60 minutes | 1,000,000,000 | The invocation ends and the script moves on to summarize |
| map | 1,000 | 5 minutes | 100,000,000 | The invocation ends and pending jobs are cancelled |
| reduce | 5,000 | 15 minutes | 100,000,000 | The invocation ends and pending jobs are cancelled |
| summarize | 10,000 | 60 minutes | 1,000,000,000 | The script stops |

- Going over an invocation limit raises `SSS_USAGE_LIMIT_EXCEEDED`.
- In map or reduce, jobs that are already running when the limit is hit normally run to completion;
  it is the pending jobs that get cancelled.
- Map and reduce let you configure retries; see
  [interruption handling](map_reduce.interruptions.md).
- Points of comparison: the map usage limit equals the limit of a mass update script; the instruction
  limit of map and reduce equals a user event script's; and the instruction limit of getInputData
  and summarize equals a scheduled script's.
- Scripts and workflows that your script sets off (for example by saving a record) do not count
  toward these limits.

## Persisted data limit (hard)

- One map/reduce execution may hold at most 200 MB of persisted data, meaning the data passed
  between stages.
- Counted: pairs not yet mapped, pairs not yet reduced, and pairs written as reduce output.
- Not counted: pairs already processed, and internal data kept for troubleshooting or analytics.
- Exceeding the limit raises `PERSISTED_DATA_LIMIT_FOR_MAPREDUCE_SCRIPT_EXCEEDED`: the invocation
  ends, the script leaves the current stage and goes to summarize. The summarize stage never runs
  into this limit.
- When getInputData returns a search, every column you include adds to the persisted data, so return
  only the columns you need.
- If you run into this limit, the suggested workaround is to store the data in a custom field or a
  custom record instead of passing it through the stages.

## Key and value size

- A key may be at most 3,000 characters; a longer key raises `KEY_LENGTH_IS_OVER_3000_BYTES`.

> [!SOURCE-ERROR] The limit is stated in characters, but the error code speaks of bytes. The source
> does not say which unit is enforced; with multi-byte characters the two differ, so keep keys well
> below 3,000 characters and do not count on the exact boundary.
<!-- source-error: topic:map_reduce.governance | - | key_limit_in_characters_but_error_code_says_bytes -->

- A value may be at most 10 MB; a larger one raises `VALUE_LENGTH_IS_OVER_10_MB`. The map context's
  `value` property is documented on the [MapReduceScript](../script-types/MapReduceScript.md) page
  with a different maximum; see that page.
- Carry your data in values, not in keys.

## Soft limits (yielding)

- In map and reduce, a job yields after it has used more than 10,000 usage units. The check runs
  after each invocation, so the yield happens only once the invocation that crossed the threshold has
  finished. This soft limit applies per enqueued job.
- A job also yields when it has run longer than the deployment's yield-after-minutes setting. That
  setting accepts 3 to 60 minutes and defaults to 60.
- After a yield, a new job with the same priority but a later timestamp takes over the remaining
  work. See [Map/reduce yielding](map_reduce.yielding.md).

## Practical advice

Keep every map and reduce invocation small. Crossing a soft limit only leads to a yield, but an
invocation that does too much on its own reaches a hard limit and is cut off.
