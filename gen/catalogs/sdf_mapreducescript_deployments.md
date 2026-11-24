# Catalog: sdf_mapreducescript_deployments
<!-- id: catalog:sdf_mapreducescript_deployments -->

| ID | Label | Value | Group | Tags |
|---|---|---|---|---|
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.everyxweeks` |  | integer |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.sunday` |  | boolean |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.monday` |  | boolean |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.tuesday` |  | boolean |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.wednesday` |  | boolean |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.thursday` |  | boolean |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.friday` |  | boolean |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.saturday` |  | boolean |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.enddate` |  | date |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.weekly.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.everyweekday.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.everyweekday.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.everyweekday.enddate` |  | date |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.everyweekday.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.scriptid` |  | string |  | required, xml_attribute |
| `mapreducescript.scriptdeployments.scriptdeployment.status` |  | single-select list | script_status | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.title` |  | string |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.isdeployed` |  | boolean |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.loglevel` |  | single-select list | script_loglevel | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.runasrole` |  | single-select list | generic_role | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.buffersize` |  | string |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.concurrencylimit` |  | string |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.queueallstagesatonce` |  | boolean |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.yieldaftermins` |  | string |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthlydayofweek.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthlydayofweek.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthlydayofweek.orderofweek` |  | single-select list | generic_order_of_week | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthlydayofweek.dayofweek` |  | single-select list | generic_day_of_week | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthlydayofweek.everyxmonths` |  | integer |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthlydayofweek.enddate` |  | date |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthlydayofweek.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthly.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthly.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthly.everyxmonths` |  | integer |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthly.enddate` |  | date |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.monthly.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearlydayofweek.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearlydayofweek.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearlydayofweek.orderofweek` |  | single-select list | generic_order_of_week | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearlydayofweek.dayofweek` |  | single-select list | generic_day_of_week | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearlydayofweek.enddate` |  | date |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearlydayofweek.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.single.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.single.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.single.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearly.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearly.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearly.enddate` |  | date |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.yearly.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.daily.startdate` |  | date |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.daily.starttime` |  | time |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.daily.everyxdays` |  | integer |  | required, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.daily.enddate` |  | date |  | optional, xml_field |
| `mapreducescript.scriptdeployments.scriptdeployment.recurrence.daily.repeat` |  | single-select list | generic_repeat_time | optional, xml_field |
