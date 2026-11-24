# Catalog: sdf_workflowactionscript_object
<!-- id: catalog:sdf_workflowactionscript_object -->

| ID | Label | Value | Group | Tags |
|---|---|---|---|---|
| `workflowactionscript.scriptid` |  | string |  | required, xml_attribute |
| `workflowactionscript.name` |  | single-select list |  | required, xml_field |
| `workflowactionscript.scriptfile` |  | filereference |  | required, xml_field |
| `workflowactionscript.returnrecordtype` |  | single-select list | script_returnrecordtype | conditionally_required, xml_field |
| `workflowactionscript.defaultfunction` |  | string |  | optional, xml_field |
| `workflowactionscript.description` |  | single-select list |  | optional, xml_field |
| `workflowactionscript.isinactive` |  | boolean |  | optional, xml_field |
| `workflowactionscript.notifyadmins` |  | boolean |  | optional, xml_field |
| `workflowactionscript.notifyemails` |  | string |  | optional, xml_field |
| `workflowactionscript.notifygroup` |  | string |  | optional, xml_field |
| `workflowactionscript.notifyowner` |  | boolean |  | optional, xml_field |
| `workflowactionscript.notifyuser` |  | boolean |  | optional, xml_field |
| `workflowactionscript.returntype` |  | single-select list | generic_customfield_fieldtype | optional, xml_field |
| `workflowactionscript.customplugintypes.plugintype.plugintype` |  | single-select list |  | required, xml_field |
| `workflowactionscript.libraries.library.scriptfile` |  | filereference |  | required, xml_field |
