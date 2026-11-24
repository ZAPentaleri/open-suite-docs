# Activity records
<!-- id: topic:records_guide.activities -->

Part of: [SuiteScript records guide](records_guide.md)

Activity records hold scheduled or logged work: events, calls, tasks, project work, the resources
booked against it, and the calendars that say how much time a resource has.

## Record types in this group

| Record ID | Reference page |
|---|---|
| `activity` | [activity](../gen/records/activity.md) |
| `calendarevent` | [calendarevent](../gen/records/calendarevent.md) |
| `phonecall` | [phonecall](../gen/records/phonecall.md) |
| `projecttask` | [projecttask](../gen/records/projecttask.md) |
| `resourceallocation` | [resourceallocation](../gen/records/resourceallocation.md) |
| `task` | [task](../gen/records/task.md) |
| `workcalendar` | No page: the source has no record data for it (see below) |

How each record is supported across SuiteScript, SOAP, CSV, Connect and REST is in the
[SuiteCloud record support catalog](../gen/catalogs/suitecloud_record_support.md).

## Work calendars

A work calendar exists to track a resource's working capacity: it tells NetSuite how much time the
resource can work. Any employee or vendor who is used as a project resource needs one.

The group names `workcalendar`, but the source documents no fields, sublists, search filters,
search columns, script types or supported operations for it, so this reference has no record page
for it. The source does not state its field IDs; check the record in your account's Records Browser
before you script against it.

## Example

A scheduled script that logs the subject and status of every phone call dated today, latest first.
The field IDs come from the [phonecall](../gen/records/phonecall.md) page, and `today` is one of
the values in the [search date filters catalog](../gen/catalogs/search_date_filters.md).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const execute = () => {
    const todaysCalls = search.create({
      type: search.Type.PHONE_CALL,
      filters: [['startdate', search.Operator.ON, 'today']],
      columns: [
        search.createColumn({ name: 'title' }),
        search.createColumn({ name: 'status' }),
        search.createColumn({ name: 'startdate', sort: search.Sort.DESC })
      ]
    });

    todaysCalls.run().each((row) => {
      const subject = row.getValue({ name: 'title' });
      const status = row.getText({ name: 'status' });
      log.debug({ title: `Call ${row.id}`, details: `${subject} (${status})` });
      return true;
    });
  };

  return { execute };
});
```
