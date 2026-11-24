# Catalog: sdf_suitelet_deployments
<!-- id: catalog:sdf_suitelet_deployments -->

| ID | Label | Value | Group | Tags |
|---|---|---|---|---|
| `suitelet.scriptdeployments.scriptdeployment.scriptid` |  | string |  | required, xml_attribute |
| `suitelet.scriptdeployments.scriptdeployment.status` |  | single-select list | script_status | required, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.title` |  | string |  | required, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.allemployees` |  | boolean |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.allpartners` |  | boolean |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.allroles` |  | boolean |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.auddepartment` |  | multi-select list |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.audemployee` |  | multi-select list |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.audgroup` |  | multi-select list |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.audpartner` |  | multi-select list |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.audslctrole` |  | multi-select list | generic_role | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.audsubsidiary` |  | multi-select list |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.eventtype` |  | single-select list | script_eventtype | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.isdeployed` |  | boolean |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.isonline` |  | boolean |  | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.loglevel` |  | single-select list | script_loglevel | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.runasrole` |  | single-select list | generic_role | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.links.link.linkcategory` |  | single-select list | generic_centercategory | required, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.links.link.linktasktype` |  | single-select list | suiteletdeployment_tasktype | optional, xml_field |
| `suitelet.scriptdeployments.scriptdeployment.links.link.linklabel` |  | single-select list |  | optional, xml_field |
