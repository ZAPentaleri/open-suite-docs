# Mass update script type (SS 1.0)
<!-- id: topic:ss1_api.mass_update -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is
> [MassUpdateScript](../script-types/MassUpdateScript.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## How it runs

- A mass update script runs when a user clicks **Perform Update** on a mass update.
- No other script can start it; in particular it cannot be queued with
  [`nlapiScheduleScript`](../ss1/search-scheduling-context.md).
- While it runs, the execution context reports `custommassupdate`.
- Each record it saves fires that record's user event scripts.
- If an error stops the run, the records already updated stay updated; nothing is rolled back.

## Converting

In 2.1 the per-record function becomes the `each` entry point of
[MassUpdateScript](../script-types/MassUpdateScript.md); the record type and ID that 1.0 passed as
positional arguments arrive as `params.type` and `params.id`. See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

The function receives the record type and internal ID of each record in the mass update's result
set. This one flags project tasks for review; because an error stops the whole run, it skips
records it does not expect instead of throwing. The custom field ID is illustrative.

```js
function flagTaskForReview(recType, recId) {
  if (recType !== 'projecttask') {
    nlapiLogExecution('DEBUG', 'Skipped unexpected type', recType + ' ' + recId);
    return;
  }
  nlapiSubmitField(recType, recId, 'custevent_needs_review', 'T');
}
```
