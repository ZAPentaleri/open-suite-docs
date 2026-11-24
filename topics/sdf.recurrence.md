# SDF scheduled deployment recurrence
<!-- id: topic:sdf.recurrence -->

Part of: [SDF scheduledscript object](sdf.scheduledscript.md)

See also: [SDF mapreducescript object](sdf.mapreducescript.md) (its deployment catalog has the same
`recurrence` structure), [SDF custom object files](sdf.object_files.md) (date and time formats),
[catalog:sdf_scheduledscript_deployments](../gen/catalogs/sdf_scheduledscript_deployments.md),
[catalog:sdf_mapreducescript_deployments](../gen/catalogs/sdf_mapreducescript_deployments.md).

When a deployment runs on a timetable, the `recurrence` element inside its `scriptdeployment`
holds that timetable. This page explains how the element is built.

## Exactly one kind per deployment

`recurrence` is a selection group: it contains a single child element, and the name of that child
is the kind of schedule. Each kind is a field group describing one scheduled script event. You
therefore pick one kind per deployment; combining two kinds in one `recurrence` is not allowed.

| Kind | Meaning | Required fields |
|---|---|---|
| `single` | One run | `startdate`, `starttime` |
| `daily` | Every N days | `everyxdays`, `startdate`, `starttime` |
| `everyweekday` | Each weekday | `startdate`, `starttime` |
| `weekly` | Every N weeks, on the days you flag | `everyxweeks`, `startdate`, `starttime`, plus the day flags `sunday` through `saturday` |
| `monthly` | Every N months | `everyxmonths`, `startdate`, `starttime` |
| `monthlydayofweek` | Every N months on a weekday position, such as the first Wednesday | `orderofweek`, `dayofweek`, `everyxmonths`, `startdate`, `starttime` |
| `yearly` | Once a year | `startdate`, `starttime` |
| `yearlydayofweek` | Once a year on a weekday position | `orderofweek`, `dayofweek`, `startdate`, `starttime` |

The required fields for `single`, `daily`, `weekly` and `monthlydayofweek` are stated in the topic
itself; those for `everyweekday`, `monthly`, `yearly` and `yearlydayofweek` come from the
deployment catalogs.

Optional fields, per the catalogs: every kind except `single` may add an `enddate` (a `single` run
never takes one), and every kind, `single` included, may add a `repeat`.

The source does not state how a `monthly` schedule picks its day of the month, nor which month a
`yearly` or `yearlydayofweek` schedule runs in: neither catalog has a field for it. Verify the
resulting schedule in the account after deploying.

## Writing the values

- **Dates and times** follow the SDF value formats: a date is `yyyy-MM-dd`; a time is `HH:mm:00Z`
  (UTC) or `HH:mm:00` (no zone). The seconds part is always `00`. Times given in UTC are converted to
  the account's time zone. Details in
  [SDF custom object files](sdf.object_files.md#writing-values).
- **`everyxdays`, `everyxweeks` and `everyxmonths`** are integers between 1 and 1000.
- **Weekday flags** in `weekly` are separate booleans, one per day, written `T` or `F`. A flag left
  at its default is `F`, so only the days you want need `T`. The catalogs nevertheless mark all seven
  flags as required, so the safe course is to write all seven.
- **`orderofweek`** is one of `FIRST`, `SECOND`, `THIRD`, `FOURTH`, `LAST`
  ([generic_order_of_week](../gen/catalogs/sdf_list_generic_order_of_week.md)). The topic infers
  this set from prose and examples rather than stating it; the value list has the same five.
- **`dayofweek`** is an upper-case day name such as `WEDNESDAY`
  ([generic_day_of_week](../gen/catalogs/sdf_list_generic_day_of_week.md)).
- **`repeat`** sets an interval at which the run is repeated, written as an ISO 8601
  duration such as `PT15M` or `PT2H`. That format is inferred from examples, not stated as a rule;
  the allowed values are in [generic_repeat_time](../gen/catalogs/sdf_list_generic_repeat_time.md).
- **`enddate`** is a plain date in the same `yyyy-MM-dd` format as `startdate`.

> [!SOURCE-ERROR] The end dates in the source's examples end in a `Z`, although a plain
> `yyyy-MM-dd` date has no zone suffix. Write end dates without the `Z`, as the examples below do,
> and confirm with a validation run.
<!-- source-error: topic:sdf.recurrence | - | example_end_dates_carry_z_suffix -->

## Examples

All dates and times below are invented.

### One run

A single run at 03:15 UTC on 9 August 2027, for example a one-time data correction.

```xml
<recurrence>
  <single>
    <startdate>2027-08-09</startdate>
    <starttime>03:15:00Z</starttime>
  </single>
</recurrence>
```

> [!SOURCE-ERROR] The source's single-run example names a month in its description that differs
> from the month in its date value. Trust the `startdate` value, not the prose around it.
<!-- source-error: topic:sdf.recurrence | - | single_example_date_described_as_another_month -->

### Every N days

Every second day at 21:00 UTC from 10 May 2027, repeated every 4 hours, until the end of
September.

```xml
<recurrence>
  <daily>
    <everyxdays>2</everyxdays>
    <startdate>2027-05-10</startdate>
    <starttime>21:00:00Z</starttime>
    <enddate>2027-09-30</enddate>
    <repeat>PT4H</repeat>
  </daily>
</recurrence>
```

> [!SOURCE-ERROR] In the source's daily example the start time carries a non-zero seconds value, and
> the accompanying text names the wrong time zone. Keep seconds at `00` (`HH:mm:00Z`) and take the
> zone from the suffix: `Z` means UTC.
<!-- source-error: topic:sdf.recurrence | - | daily_example_start_time_has_seconds_and_wrong_zone_text -->

### Weekly on chosen days

Every week on Tuesday, Wednesday and Saturday at 07:00 with no zone given.

```xml
<recurrence>
  <weekly>
    <everyxweeks>1</everyxweeks>
    <startdate>2027-06-01</startdate>
    <starttime>07:00:00</starttime>
    <sunday>F</sunday>
    <monday>F</monday>
    <tuesday>T</tuesday>
    <wednesday>T</wednesday>
    <thursday>F</thursday>
    <friday>F</friday>
    <saturday>T</saturday>
  </weekly>
</recurrence>
```

### Monthly on a weekday position

The first Wednesday of every second month at 16:30 UTC.

```xml
<recurrence>
  <monthlydayofweek>
    <orderofweek>FIRST</orderofweek>
    <dayofweek>WEDNESDAY</dayofweek>
    <everyxmonths>2</everyxmonths>
    <startdate>2027-02-01</startdate>
    <starttime>16:30:00Z</starttime>
  </monthlydayofweek>
</recurrence>
```

### Yearly on a weekday position

`yearlydayofweek` takes the same `orderofweek` / `dayofweek` pair (for example `FOURTH` and
`THURSDAY`) but no interval. As noted above, the source gives no month field, so which month the run
lands in is not documented.

> [!SOURCE-ERROR] The source's description of its `yearlydayofweek` example says the script runs
> "every third week", which contradicts a once-a-year schedule. Treat the kind as yearly and check
> the schedule the account shows after deployment.
<!-- source-error: topic:sdf.recurrence | - | yearlydayofweek_example_described_as_every_third_week -->
