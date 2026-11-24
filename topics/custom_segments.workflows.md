# Custom segments in workflows
<!-- id: topic:custom_segments.workflows -->

Part of: [Custom segments](custom_segments.md)

See also: [SuiteFlow workflows](suiteflow.md).

Workflows can work with custom segments in two ways: as a field on the records the segment is applied
to, and on the records that hold the segment's values.

## Segment as a field

On a record that has the segment, a workflow refers to the segment just like any ordinary field.

A workflow is also an alternative to a segment's source list when you want defaults that differ by
record type. Keep in mind that a source list fills in its default only when the source field changes.

## Segment value records

The values of a segment are records of a type named after the segment, so workflows can be deployed on
them. Such a workflow runs only when a value is edited on its own form; edits made on the values
sublist do not trigger it.
