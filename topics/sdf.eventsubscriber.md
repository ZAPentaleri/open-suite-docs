# SDF eventsubscriber object
<!-- id: topic:sdf.eventsubscriber -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [EventSubscriber](../script-types/EventSubscriber.md). Value lists:
[SDF value lists for script objects](sdf.value_lists.md).

Generated catalog: [catalog:sdf_eventsubscriber_object](../gen/catalogs/sdf_eventsubscriber_object.md).
This object has no separate deployment or script-parameter catalog.

An `eventsubscriber` object ties an event subscriber script to the record events it listens for.
Its shape differs from the other script objects: instead of a `scriptdeployments` structure, it has
`criteria` that pick the events which start the script.

## Identity and the script file

- An event subscriber script has exactly one `eventsubscriber` object; you cannot split one script
  across several objects.
- The script ID begins with `custevsub`, may be at most 27 characters long, and has to be the same
  as the name of the object's XML file.
- The script file is pointed to by `scriptfilepath`, not by the `scriptfile` element that the other
  script objects use.

## Criteria

The `criteria` element is mandatory. Each `criterion` inside it describes a group of events that
trigger the script, and each criterion must carry `parameters` that narrow it down.

| Element | Values accepted |
|---|---|
| `criterion.domain` | `RECORD` |
| `criterion.type` | `CREATED`, `UPDATED`, `DELETED` |
| `criterion.parameters` | Required; contains `parameter` elements |
| `parameter.name` | `RECORDTYPE` |
| `parameter.operator` | `EQ` |
| `parameter.value` | Either a standard record type ID (see [generic_standard_recordtype](../gen/catalogs/sdf_list_generic_standard_recordtype.md)) or the script ID of a custom record type |

Read together, one criterion amounts to "run when a record of this type is created, updated or
deleted". These are the only domain, type, name and operator values the source lists.

The source does not state in which form a custom record type must be written in
`parameter.value`: as its bare script ID or as a bracketed `[scriptid=...]` reference. Check an
object exported from an account before relying on either.

## Object fields

Source: [catalog:sdf_eventsubscriber_object](../gen/catalogs/sdf_eventsubscriber_object.md). Paths
start at the `eventsubscriber` element.

| Element | Value type | Value list | Use | Notes |
|---|---|---|---|---|
| `scriptid` | string | | Required | XML attribute; prefix `custevsub`; max 27 characters; equals the XML file name |
| `scriptfilepath` | filereference | | Required | The script file |
| `criteria.criterion.domain` | string | | Required | `RECORD` |
| `criteria.criterion.type` | string | | Required | `CREATED`, `UPDATED` or `DELETED` |
| `criteria.criterion.parameters.parameter.name` | string | | Required | `RECORDTYPE` |
| `criteria.criterion.parameters.parameter.operator` | string | | Required | `EQ` |
| `criteria.criterion.parameters.parameter.value` | string | [generic_standard_recordtype](../gen/catalogs/sdf_list_generic_standard_recordtype.md) | Required | Or the script ID of a custom record type |

## Example

A subscriber for a returns team: it runs when a return authorization is deleted and when a sales
order is updated. IDs and paths are invented; the script ID stays within 27 characters, and the
file would be saved as `custevsub_rt_rma_audit.xml`.

```xml
<eventsubscriber scriptid="custevsub_rt_rma_audit">
  <scriptfilepath>[/SuiteScripts/rt_returns/rma_audit_sub.js]</scriptfilepath>
  <criteria>
    <criterion>
      <domain>RECORD</domain>
      <type>DELETED</type>
      <parameters>
        <parameter>
          <name>RECORDTYPE</name>
          <operator>EQ</operator>
          <value>RETURNAUTHORIZATION</value>
        </parameter>
      </parameters>
    </criterion>
    <criterion>
      <domain>RECORD</domain>
      <type>UPDATED</type>
      <parameters>
        <parameter>
          <name>RECORDTYPE</name>
          <operator>EQ</operator>
          <value>SALESORDER</value>
        </parameter>
      </parameters>
    </criterion>
  </criteria>
</eventsubscriber>
```
