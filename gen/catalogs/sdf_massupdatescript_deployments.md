# Catalog: sdf_massupdatescript_deployments
<!-- id: catalog:sdf_massupdatescript_deployments -->

| ID | Label | Value | Group | Tags |
|---|---|---|---|---|
| `massupdatescript.scriptdeployments.scriptdeployment.scriptid` |  | string |  | required, xml_attribute |
| `massupdatescript.scriptdeployments.scriptdeployment.status` |  | single-select list | script_status | required, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.recordtype` |  | single-select list | scriptdeployment_recordtype | required, xml_field, accepts_ref_customrecordtype, accepts_ref_customtransactiontype |
| `massupdatescript.scriptdeployments.scriptdeployment.allemployees` |  | boolean |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.allpartners` |  | boolean |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.allroles` |  | boolean |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.auddepartment` |  | multi-select list |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.audemployee` |  | multi-select list |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.audgroup` |  | multi-select list |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.audpartner` |  | multi-select list |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.audslctrole` |  | multi-select list | generic_role | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.audsubsidiary` |  | multi-select list |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.isdeployed` |  | boolean |  | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.loglevel` |  | single-select list | script_loglevel | optional, xml_field |
| `massupdatescript.scriptdeployments.scriptdeployment.runasrole` |  | single-select list | generic_role | optional, xml_field |
