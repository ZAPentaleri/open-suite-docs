# Catalog: sdf_usereventscript_object
<!-- id: catalog:sdf_usereventscript_object -->

| ID | Label | Value | Tags |
|---|---|---|---|
| `usereventscript.scriptid` |  | string | required, xml_attribute |
| `usereventscript.name` |  | single-select list | required, xml_field |
| `usereventscript.scriptfile` |  | filereference | required, xml_field |
| `usereventscript.description` |  | single-select list | optional, xml_field |
| `usereventscript.isinactive` |  | boolean | optional, xml_field |
| `usereventscript.notifyadmins` |  | boolean | optional, xml_field |
| `usereventscript.notifyemails` |  | string | optional, xml_field |
| `usereventscript.notifygroup` |  | string | optional, xml_field |
| `usereventscript.notifyowner` |  | boolean | optional, xml_field |
| `usereventscript.notifyuser` |  | boolean | optional, xml_field |
| `usereventscript.aftersubmitfunction` |  | string | optional, xml_field |
| `usereventscript.beforeloadfunction` |  | string | optional, xml_field |
| `usereventscript.beforesubmitfunction` |  | string | optional, xml_field |
| `usereventscript.libraries.library.scriptfile` |  | filereference | required, xml_field |
| `usereventscript.customplugintypes.plugintype.plugintype` |  | single-select list | required, xml_field |
