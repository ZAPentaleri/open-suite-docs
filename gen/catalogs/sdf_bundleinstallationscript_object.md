# Catalog: sdf_bundleinstallationscript_object
<!-- id: catalog:sdf_bundleinstallationscript_object -->

| ID | Label | Value | Tags |
|---|---|---|---|
| `bundleinstallationscript.scriptid` |  | string | required, xml_attribute |
| `bundleinstallationscript.name` |  | single-select list | required, xml_field |
| `bundleinstallationscript.scriptfile` |  | filereference | required, xml_field |
| `bundleinstallationscript.description` |  | single-select list | optional, xml_field |
| `bundleinstallationscript.isinactive` |  | boolean | optional, xml_field |
| `bundleinstallationscript.notifyadmins` |  | boolean | optional, xml_field |
| `bundleinstallationscript.notifyemails` |  | string | optional, xml_field |
| `bundleinstallationscript.notifyowner` |  | boolean | optional, xml_field |
| `bundleinstallationscript.afterinstallfunction` |  | string | optional, xml_field |
| `bundleinstallationscript.afterupdatefunction` |  | string | optional, xml_field |
| `bundleinstallationscript.beforeinstallfunction` |  | string | optional, xml_field |
| `bundleinstallationscript.beforeuninstallfunction` |  | string | optional, xml_field |
| `bundleinstallationscript.beforeupdatefunction` |  | string | optional, xml_field |
| `bundleinstallationscript.customplugintypes.plugintype.plugintype` |  | single-select list | required, xml_field |
| `bundleinstallationscript.libraries.library.scriptfile` |  | filereference | required, xml_field |
