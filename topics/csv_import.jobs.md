# CSV import jobs, status and preferences
<!-- id: topic:csv_import.jobs -->

Part of: [CSV import](csv_import.md)

What happens once a mapping is ready: saving and starting a job, how jobs wait their turn, what the
job status page shows, cancelling, where errors come from, slow imports, and the account-wide CSV
import preferences.

## Saving and starting

The last assistant step offers five actions: Save, Run, Save & Run, Save As, and Save As & Run.

A saved map gets a script ID that begins with `CUSTIMPORT`. The part you add may contain only
letters, digits and underscores, and the whole ID must be unique in the account. (Whether a default
ID is filled in when you leave it blank is disputed in the source; see
[saved maps](csv_import.saved_imports.md).)

A single file can hold at most 25,000 records.

## Scripted jobs

A script submits the same kind of job with
[`CsvImportTask`](../modules/N_task.md#csvimporttask-object), pointing it at a saved map by ID.

> [!SOURCE-ERROR] Where the source describes running imports with the 2.x `CsvImportTask`, it
> refers to the SuiteScript 1.0 `nlobjCSVImport` object, mixing the two API versions. In 2.x code
> use `N/task` and `CsvImportTask`; `nlobjCSVImport` belongs to 1.0 scripts only.
<!-- source-error: topic:csv_import.jobs | - | csvimporttask_context_mentions_nlobjcsvimport_object -->

## Queueing

Import jobs queue per company: a new job stays Pending until the jobs submitted before it have
finished. With a SuiteCloud Plus license, jobs can be spread across several queues; see
[threads and queues](csv_import.multithreading.md).

## Job status

| Status | Meaning |
|---|---|
| Pending | Waiting for earlier jobs to finish. |
| Processing | Running. |
| Completed | Finished; the results file lists any rows that failed. |
| Canceled | Stopped by a user. |
| Retry | Started again after an infrastructure problem. |

- If you do not name a job, its name is built from the import type, the file name and the user's
  email address.
- Every job produces a results CSV holding the rows that failed or were never processed.
- Rows on the status page are kept for 30 days, with a ceiling of 1,000,000 jobs. You cannot delete
  them yourself.
- The completion email cannot be switched off.

### When a user event fails after save

If an `afterSubmit` user event script fails, the record it ran on has already been saved. The row is
still reported with an error, but importing it again is the wrong fix; rerun the script's work for
that record instead.

## Cancelling

- Cancel jobs from the CSV import status page under Setup > Import/Export.
- Only Pending and Processing jobs can be cancelled.
- Administrators can cancel anyone's job; other users can cancel only their own.
- While the cancel is in progress the job shows a Canceling status.
- As soon as a job is cancelled, the next job in the queue starts.
- A Pending job that you cancel disappears from the status page.
- When cancelling several jobs, start with the one submitted last and work backwards.

When a job is cancelled while Processing:

- its status becomes Canceled;
- the status message shows how many rows were imported before the cancel;
- the results CSV holds the rows that failed and the rows that were never reached;
- an email with the counts goes to the address the user signed in with;
- the count of rows not imported includes the unprocessed rows, not just the failed ones.

## Where errors come from

Errors can appear at three stages: while the upload is validated, during field mapping, and in
post-processing after the rows are imported. Individual messages are listed in the error sub-topics
([general](csv_import.errors_general.md), [items](csv_import.errors_items.md),
[items, continued](csv_import.errors_items_more.md), [transactions](csv_import.errors_transactions.md)).
Some recurring ones:

- Giving a country as a code instead of its name fails with an invalid country reference key error;
  use country names (see [file conventions](csv_import.file_conventions.md)).
- Subsidiary values must spell out the full hierarchy, with the levels separated by a colon.
- Script usage limit errors in the results come from scripts the import triggered, not from the
  import itself.
- An `afterSubmit` script failure still leaves the record saved (see above).
- Imported records cannot be mass-deleted afterwards; delete them one at a time.

## Slow imports

If imports run slowly:

- avoid the account's busy hours;
- save the job and run it later;
- split the data into smaller files and separate jobs;
- for project data and employee entry data, let project plans be recalculated asynchronously.

## Account-wide CSV import preferences

The account preferences set the starting value of these options:

- multi-select delimiter,
- custom field system notes,
- running server SuiteScript and triggering workflows,
- column delimiter,
- decimal delimiter.

Changing them requires the `Set Up CSV Preferences` permission (`ADMI_CSVIMPORTPREF`; see
[permissions](../gen/catalogs/permissions.md)). They do not apply to the simple two-step imports,
and a value set on an individual job overrides them (see
[advanced options](csv_import.advanced_options.md)).

> [!SOURCE-ERROR] The source's preferences page includes colon among the column delimiters, while
> other pages list only comma, semicolon, pipe, space and tab. The source does not say which list is
> right; check the choices offered in your account before relying on a colon.
<!-- source-error: topic:csv_import.jobs | - | column_delimiter_list_adds_colon -->

### Running scripts during imports

- For ongoing imports that keep NetSuite in step with another system, let server SuiteScript run.
- For one-time loads of historical data, the source recommends skipping scripts.
- Changing the run-server-SuiteScript option on a job requires both `Import CSV File`
  (`ADMI_IMPORTCSVFILE`) and `Control SuiteScript and Workflow Triggers per CSV Import`
  (`ADMI_IMPORTOVERRIDESSTRIG`), each at the Full level.
