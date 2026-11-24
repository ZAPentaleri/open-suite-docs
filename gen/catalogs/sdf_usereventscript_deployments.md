# Catalog: sdf_usereventscript_deployments
<!-- id: catalog:sdf_usereventscript_deployments -->

| ID | Label | Value | Group | Tags |
|---|---|---|---|---|
| `usereventscript.scriptdeployments.scriptdeployment.scriptid` |  | string |  | required, xml_attribute |
| `usereventscript.scriptdeployments.scriptdeployment.status` |  | single-select list | script_status | required, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.recordtype` |  | single-select list | generic_standard_recordtype | required, xml_field, also_list_allrecord_script_deployment_recordtype, accepts_ref_customrecordtype, accepts_ref_customtransactiontype |
| `usereventscript.scriptdeployments.scriptdeployment.allemployees` |  | boolean |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.allpartners` |  | boolean |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.allroles` |  | boolean |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.auddepartment` |  | multi-select list |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.audemployee` |  | multi-select list |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.audgroup` |  | multi-select list |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.audpartner` |  | multi-select list |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.audslctrole` |  | multi-select list | generic_role | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.audsubsidiary` |  | multi-select list |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.eventtype` |  | single-select list | script_eventtype | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.isdeployed` |  | boolean |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.loglevel` |  | single-select list | script_loglevel | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.runasrole` |  | single-select list | generic_role | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.alllocalizationcontexts` |  | boolean |  | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.executioncontext` |  | multi-select list | execution_context | optional, xml_field |
| `usereventscript.scriptdeployments.scriptdeployment.localizationcontext` |  | multi-select list | countries | optional, xml_field |
