# Catalog: sdf_workflowactionscript_deployments
<!-- id: catalog:sdf_workflowactionscript_deployments -->

| ID | Label | Value | Group | Tags |
|---|---|---|---|---|
| `workflowactionscript.scriptdeployments.scriptdeployment.scriptid` |  | string |  | required, xml_attribute |
| `workflowactionscript.scriptdeployments.scriptdeployment.status` |  | single-select list | script_status | required, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.recordtype` |  | single-select list | scriptdeployment_recordtype | required, xml_field, also_list_allrecord_script_deployment_recordtype, accepts_ref_customrecordtype, accepts_ref_customtransactiontype |
| `workflowactionscript.scriptdeployments.scriptdeployment.allemployees` |  | boolean |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.allpartners` |  | boolean |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.allroles` |  | boolean |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.auddepartment` |  | multi-select list |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.audemployee` |  | multi-select list |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.audgroup` |  | multi-select list |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.audpartner` |  | multi-select list |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.audslctrole` |  | multi-select list | generic_role | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.audsubsidiary` |  | multi-select list |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.isdeployed` |  | boolean |  | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.loglevel` |  | single-select list | script_loglevel | optional, xml_field |
| `workflowactionscript.scriptdeployments.scriptdeployment.runasrole` |  | single-select list | generic_role | optional, xml_field |
