# CSV import threads and queues
<!-- id: topic:csv_import.multithreading -->

Part of: [CSV import](csv_import.md)

Two advanced options let large imports run faster: *Use multi-threading* splits one job across
threads, and *Queue number* lets separate jobs run side by side. Both need a SuiteCloud Plus
license.

## Queues

- An account has at most 5 import queues, numbered 1 to 5. The queue number defaults to 1.
- Jobs in the same queue run one after another; jobs in different queues run in parallel.
- Once a job is submitted its queue cannot be changed; cancel it and submit it again on the other
  queue.
- When a saved import map is installed from a bundle, its queue number is reset to 1.
- Scripts choose the queue per job with
  [`CsvImportTask.queueId`](../modules/N_task.md#csvimporttaskqueueid).

## Multi-threading

- Off by default.
- One SuiteCloud Plus license gives a job 2 threads; with more licenses a job can use up to 10.
- The setting is kept in saved import maps, and in maps distributed in bundles.
- Rows are not guaranteed to be processed in file order.
- Rows that depend on other rows in the same file (for example, one record referring to another
  created earlier in the file) can fail when the rows land on different threads. Rerun the job to
  pick them up.

### Keep each record's rows together

With multi-threading on and a sublist laid out as consecutive rows, all rows of one record must sit
next to each other in the file. Sublists laid out as numbered columns behave differently: their rows
are submitted separately. See [single-file layout of sublist rows](csv_import.single_file_sublists.md)
for the two layouts.

## Scripted imports

Imports submitted through [`CsvImportTask`](../modules/N_task.md#csvimporttask-object) run on a
single thread in a single queue; the queue and threading settings saved on the map are not used
(see the [N/task page](../modules/N_task.md#csvimporttask-object)).
