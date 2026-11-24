# Where custom segments apply
<!-- id: topic:custom_segments.application -->

Part of: [Custom segments](custom_segments.md)

A custom segment is applied to a set of record types, and several of its other settings constrain that
set. The settings that affect where a segment applies are: dynamic defaults, filtering by another
segment, filtering by a classification, GL impact, and type constraints.

## Dependencies between segments

- **Filtered by another segment**: the filtering segment must be applied to the same record types as
  the segment it filters.
- **Source list**: the record type behind the source list must itself have the segment applied.

In both cases, if the condition is not met, the system offers to extend the segment's application for
you.

## GL impact

- A segment with GL impact shows up on the GL impact page; this only concerns transactions.
- The GL impact setting cannot be changed once the segment has been created.

## Multiple-select segments

A multiple-select segment cannot be applied to transaction sublists.

## Filtering by classification

- A segment can be filtered by class, department, location or subsidiary.
- Custom fields that happen to be named like one of these classifications cannot be used for this
  filtering.
- Filtering by class, department or location is not available for CRM records, other record types,
  custom record types or custom segments.
- Filtering by subsidiary is not available for custom record types or custom segments.
