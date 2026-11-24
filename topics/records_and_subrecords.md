# Scripting records and subrecords
<!-- id: topic:records_and_subrecords -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

Sub-topics:

- [Subrecord scripting basics](records_and_subrecords.subrecord_basics.md)
- [Body field subrecords](records_and_subrecords.body_subrecords.md)
- [Sublist subrecords](records_and_subrecords.sublist_subrecords.md)

See also: [Record Modes](record_modes.md), [N/record](../modules/N_record.md),
[N/currentRecord](../modules/N_currentRecord.md).

## Two modules for record work

SuiteScript gives you two modules for reading and changing records:

- [N/record](../modules/N_record.md) is what server scripts use for all record work.
- Client scripts use [N/currentRecord](../modules/N_currentRecord.md) for the record open in the
  browser, and [N/record](../modules/N_record.md) when they need to reach other (remote) records.

## Standard and dynamic mode

A record object works in one of two modes, standard or dynamic. The mode decides when sourcing,
validation and calculation happen and whether the order in which you set fields matters. See
[Record Modes](record_modes.md).

## What a subrecord is

A subrecord holds detail that belongs to a parent record. You never open it on its own: you reach it
through a summary field on the parent, which can sit in the body or on a sublist line. Common
subrecords are addresses ([address](../gen/records/address.md)), order schedules
([orderschedule](../gen/records/orderschedule.md)) and inventory detail
([inventorydetail](../gen/records/inventorydetail.md)).

The sub-topics explain how to reach, create, edit and read subrecords in each placement.
