# Catalog: sdf_scheduledscript_object
<!-- id: catalog:sdf_scheduledscript_object -->

| ID | Label | Value | Tags |
|---|---|---|---|
| `scheduledscript.scriptid` |  | string | required, xml_attribute |
| `scheduledscript.name` |  | single-select list | required, xml_field |
| `scheduledscript.scriptfile` |  | filereference | required, xml_field |
| `scheduledscript.defaultfunction` |  | string | optional, xml_field |
| `scheduledscript.description` |  | single-select list | optional, xml_field |
| `scheduledscript.isinactive` |  | boolean | optional, xml_field |
| `scheduledscript.notifyadmins` |  | boolean | optional, xml_field |
| `scheduledscript.notifyemails` |  | string | optional, xml_field |
| `scheduledscript.notifygroup` |  | string | optional, xml_field |
| `scheduledscript.notifyowner` |  | boolean | optional, xml_field |
| `scheduledscript.libraries.library.scriptfile` |  | filereference | required, xml_field |
| `scheduledscript.customplugintypes.plugintype.plugintype` |  | single-select list | required, xml_field |
