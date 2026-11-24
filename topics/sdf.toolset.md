# SDF toolset object
<!-- id: topic:sdf.toolset -->

Part of: [SuiteCloud Development Framework](sdf.md)

Script type: [CustomTool](../script-types/CustomTool.md). Related: [Other SDF objects used with
scripts](sdf.other_objects.md), [Custom Tools Page](custom_tool.management.md),
[SDF value lists](sdf.value_lists.md).

A `toolset` object is the SDF object type through which [custom tool](../script-types/CustomTool.md)
scripts reach an account: one object defines a set of custom tools. There is no generated element
catalog for this object type; the facts below come from the topic itself.

## Requirements and history

- The project manifest must declare the `SERVERSIDESCRIPTING` feature as a dependency before a
  `toolset` object can be deployed (see [SDF manifest dependencies](sdf.manifest_dependencies.md)).
- Release 2026.1 changed the object definition so that custom tools can write execution logs.
  Toolsets created before that change have to be brought in line with the new definition.

## Identity

- `scriptid` is the object's only XML attribute, and it is required.
- It begins with `custtoolset` (also the value the field starts out with) and is at most 27
  characters long.
- Only lower-case letters, digits and underscores are allowed, and the ID must be unique.
- The script ID doubles as the file name of the object's XML file.

## Elements

| Element | Type | Use | Notes |
|---|---|---|---|
| `scriptid` | string | Required | XML attribute; prefix `custtoolset`; at most 27 characters |
| `name` | string | Required | The source gives no length limit |
| `scriptfile` | filereference | Required | Must point at a JavaScript (`.js`) file: the tools' implementation |
| `rpcschema` | filereference | Required | Must point at a `.json` file holding the tools' JSON-RPC schema |
| `exposetoaiconnector` | boolean | Optional | Defaults to `F`; `T` makes the toolset available through the NetSuite AI Connector Service |
| `permissions` | structure | Optional | Contains `permission` elements (see below) |

## Permissions and tool visibility

`permissions` is the object's only nested structure. It contains `permission` elements, and each
`permission` has two fields:

| Element (under `permissions.permission`) | Type | Use | Values |
|---|---|---|---|
| `permkey` | single-select list | Required in every `permission` | The permission to check: an entry of the `generic_permission` value list, or a reference to a `customtransactiontype`, `customsegment` or `customrecordtype` object |
| `permlevel` | single-select list | Required in every `permission` | The level of that permission: an entry of the `generic_permission_level` value list |

- Once a `permission` element is present, both `permkey` and `permlevel` must be filled in.
- The accepted IDs are the entries of the two value lists:
  [sdf_list_generic_permission](../gen/catalogs/sdf_list_generic_permission.md) for `permkey`
  (for example `TRAN_PURCHREQ`) and
  [sdf_list_generic_permission_level](../gen/catalogs/sdf_list_generic_permission_level.md) for
  `permlevel` (`CREATE`, `EDIT`, `FULL`, `NONE`, `VIEW`). The source lists these IDs only: it does
  not state what each permission ID means or which levels a given permission accepts.

What the list does: an AI client shows a tool from this toolset only to users who hold **every**
listed permission. It is a visibility check, not a grant. Once a user can see and call a tool,
the data the tool can read and the actions it can take are still limited by that user's role.

## Example

A toolset that lets an AI client look up open purchase requisitions. It is offered through the AI
Connector Service, and its tools appear only for users who hold both listed permissions: one
built-in key and one reference to a custom record type in the same project. File names, paths and
IDs are invented; the `permkey` and `permlevel` values are entry IDs from the generated lists,
whose meanings the source does not give. Saved as `custtoolset_req_lookup.xml`.

```xml
<toolset scriptid="custtoolset_req_lookup">
  <name>Requisition lookup tools</name>
  <scriptfile>[/SuiteScripts/proc_tools/req_lookup_tools.js]</scriptfile>
  <rpcschema>[/SuiteScripts/proc_tools/req_lookup_schema.json]</rpcschema>
  <exposetoaiconnector>T</exposetoaiconnector>
  <permissions>
    <permission>
      <permkey>TRAN_PURCHREQ</permkey>
      <permlevel>VIEW</permlevel>
    </permission>
    <permission>
      <permkey>[scriptid=customrecord_proc_req_queue]</permkey>
      <permlevel>VIEW</permlevel>
    </permission>
  </permissions>
</toolset>
```

The `[scriptid=...]` reference form is described in
[SDF custom object files](sdf.object_files.md#referencing-other-objects-and-files).

The matching manifest entry for the feature dependency:

```xml
<dependencies>
  <features>
    <feature required="true">SERVERSIDESCRIPTING</feature>
  </features>
</dependencies>
```
