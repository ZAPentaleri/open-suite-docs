# Catalog: sdf_clientscript_deployments
<!-- id: catalog:sdf_clientscript_deployments -->

| ID | Label | Value | Group | Tags |
|---|---|---|---|---|
| `clientscript.scriptdeployments.scriptdeployment.scriptid` |  | string |  | required, xml_attribute |
| `clientscript.scriptdeployments.scriptdeployment.status` |  | single-select list | script_status | required, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.recordtype` |  | single-select list | scriptdeployment_recordtype | required, xml_field, also_list_allrecord_script_deployment_recordtype, accepts_ref_customrecordtype, accepts_ref_customtransactiontype |
| `clientscript.scriptdeployments.scriptdeployment.allemployees` |  | boolean |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.allpartners` |  | boolean |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.allroles` |  | boolean |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.auddepartment` |  | multi-select list |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.audemployee` |  | multi-select list |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.audgroup` |  | multi-select list |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.audpartner` |  | multi-select list |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.audslctrole` |  | multi-select list | generic_role | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.audsubsidiary` |  | multi-select list |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.eventtype` |  | single-select list | script_eventtype | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.isdeployed` |  | boolean |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.loglevel` |  | single-select list | script_loglevel | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.alllocalizationcontexts` |  | boolean |  | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.executioncontext` |  | multi-select list | execution_context | optional, xml_field |
| `clientscript.scriptdeployments.scriptdeployment.localizationcontext` |  | multi-select list | countries | optional, xml_field |
