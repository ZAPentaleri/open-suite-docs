# Work Order (`workorder`)
<!-- id: record:workorder -->

| Fact | Value |
|---|---|
| Internal ID | `workorder` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Search data via | Transaction |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `assemblyitem`, `assemblyitem_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `requires_feature`: `Assembly Items and Work Orders` [`real_name`]
- `purpose`: `track_assembly_production_and_components`
- `requires`: `allow_purchase_of_assembly_items_off`
- `difference`: `special_order_tied_to_sale_production_for_stock`
- `effect`: `build_point_adds_to_mass_create_work_orders_queue`
- `sublist_requires_feature`: `partners:multi_partner_management`

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `actualproductionenddate` | date | Actual Production End Date | no | no |  | `purpose`: `actual_production_end_date`; `value_source`: `latest_completion_or_build_date` |
| `actualproductionstartdate` | date | Actual Production Start Date | no | no |  | `purpose`: `actual_production_start_date`; `value_source`: `earliest_issue_completion_or_build_date`; `can`: `override_with_enter_manually` |
| `assemblyitem` | select | Assembly | yes | no |  | `purpose`: `assembly_item`; `ui_effect`: `components_listed_on_items_subtab`; `cannot`: `change_after_assembly_build_linked` |
| `autocalculatelag` | checkbox | Auto-calculate Lag | no | no |  | `purpose`: `auto_calculate_lag` |
| `billofmaterials` | select | Bill of Materials | no | no |  | `purpose`: `bill_of_materials`; `value_source`: `auto_selected` (when `advanced_bills_of_materials`) |
| `billofmaterialsrevision` | select | Bill of Materials Revision | no | no |  | `purpose`: `bom_revision`; `value_source`: `revision_effective_on_production_start` (when `advanced_bills_of_materials`) |
| `class` | select | Class | no | no |  | `purpose`: `class`; `effect`: `carried_to_assembly_build`; `ui_effect`: `new_option_creates_record` |
| `createdfrom` | select | Created From | no | no |  | `purpose`: `created_from_sales_order`; `value_source`: `system_displayed`; `shown_only_when`: `created_from_sales_order` |
| `customform` | select | Custom Form | yes | no |  | `purpose`: `custom_form` |
| `department` | select | Department | no | no |  | `purpose`: `department`; `effect`: `carried_to_assembly_build`; `ui_effect`: `new_option_creates_record` |
| `enddate` | date | Production End Date | no | no |  | `purpose`: `production_end_date`; `default_behavior`: `start_date_plus_lead_time`; `value_source`: `computed` (when `forward_scheduling`) |
| `entity` | select | Customer | no | no |  | `purpose`: `customer` |
| `expandassembly` | checkbox | Build Sub-Assemblies | no | no |  | `purpose`: `mark_subassemblies_phantom`; `effect`: `subassembly_components_listed` |
| `firmed` | checkbox | Firmed | no | no |  | `purpose`: `firmed`; `true_means`: `kept_through_supply_planning`; `false_means`: `deleted_before_supply_planning` |
| `job` | select | Project | no | no |  | `purpose`: `job` |
| `location` | select | Location | no | no |  | `purpose`: `location`; `effect`: `components_committed_from_location` (when `multi_location_inventory`); `effect`: `shown_on_assembly_build` |
| `memo` | text | Memo | no | yes |  | `purpose`: `memo`; `supported_in`: `search` |
| `orderstatus` | text | Status | no | no |  | `purpose`: `work_order_status`; `value_examples`: `In-Process`, `Built` |
| `quantity` | posfloat | Quantity | yes | no |  | `purpose`: `quantity_to_build`; `accepts_forms`: `fractional_number` |
| `requesteddate` | date | Supply Required By Date | no | no |  | `purpose`: `supply_required_by_date` |
| `revision` | select | Revision | no | no |  | `purpose`: `revision`; `default_behavior`: `effective_revision`; `effect`: `change_replaces_top_level_components`; `caveat`: `lower_levels_by_effective_date_when_building_subassemblies` |
| `salesgroup` | select | Choose Team | no | no |  | `purpose`: `sales_team` |
| `schedulingmethod` | select | Scheduling Method | yes | no |  | `purpose`: `scheduling_method`; `options`: `Forward Scheduling`, `Backward Scheduling`; `option_meanings`: `forward:start_given_end_calculated`, `backward:end_given_start_calculated` |
| `startdate` | date | Production Start Date | no | no |  | `purpose`: `production_start_date`; `default_behavior`: `current_date`; `stage_order`: `location_time_zone`, `subsidiary_time_zone`, `company_time_zone` |
| `status` | text | Status | no | no |  | `purpose`: `operation_status`; `value_examples`: `Not Started`, `Started`, `Completed` |
| `subsidiary` | select | Subsidiary | yes | no |  | `purpose`: `subsidiary`; `requires_feature`: `OneWorld` [`real_name`] |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  | `purpose`: `sync_partner_team_to_customer` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  | `purpose`: `sync_sales_team_to_customer` |
| `trandate` | date | Date | yes | no |  | `purpose`: `work_order_date`; `default_behavior`: `current_date` |
| `tranid` | text | Order # | no | no |  | `purpose`: `work_order_number`; `default_behavior`: `highest_number_plus_one`; `caveat`: `next_number_reverts_to_standard_pattern` |
| `units` | select | Units | no | no |  | `purpose`: `component_units`; `value_source`: `system_displayed` |
| `createddate` | datetime |  | no | no |  |  |
| `currency` | text |  | no | no |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |
| `exchangerate` | text |  | no | no |  |  |
| `externalid` | text | ExternalId | no | no |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |
| `nexus` | select | Nexus | no | no |  |  |
| `revisionmemo` | textarea | Revision Memo | no | no |  |  |
| `salesrep` | select | Sales Rep | no | yes |  |  |
| `source` | text | Source | no | no |  |  |
| `statusRef` | text |  | no | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `commitinventory` | select | Commit | no |  |  |
| `createpo` | select | Create PO | no |  |  |
| `createwo` | checkbox | Create WO | no |  |  |
| `daysbeforeexpiration` | text |  | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `id` | text |  | no |  |  |
| `inventorydetail` | summary | Inventory Detail | no |  |  |
| `isclosed` | text |  | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemid` | text |  | no |  |  |
| `itemsource` | select | Item Source | no |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `operationsequencenumber` | integer | Operation Sequence Number | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `orderallocationstrategy` | select | Allocation Strategy | no |  |  |
| `orderpriority` | float | Order Priority | no |  |  |
| `plannedissuedate` | datetime | Planned Component Issue Date | no |  |  |
| `porate` | currency2 |  | no |  |  |
| `povendor` | text |  | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `quantityavailable` | float |  | no |  |  |
| `quantitybackordered` | float | Back Ordered | no |  |  |
| `quantitycommitted` | float | Committed | no |  |  |
| `quantityfulfilled` | float | Used in Build | no |  |  |
| `rate` | rate |  | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `units` | select | Units | no |  |  |

## Sublist `partners` (Partners)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `contribution` | percent | Contribution % | yes |  |  |
| `id` | text |  | no |  |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `partner` | select | Partner | yes |  |  |
| `partnerrole` | select | Partner Role | no |  |  |
| `transaction` | text |  | no |  |  |

## Sublist `salesteam` (Sales Team)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `contribution` | percent | Contribution % | yes |  |  |
| `employee` | select | Employee | yes |  |  |
| `id` | text |  | no |  |  |
| `isprimary` | checkbox | Primary | no |  |  |
| `issalesrep` | text |  | no |  |  |
| `salesrole` | select | Sales Role | yes |  |  |
| `transaction` | text |  | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `custom` | Custom |
| `history` | History |
| `items` | Items |
| `multipartner` | Partners |
| `teamselling` | Sales Team |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `workorderclose` | `customform` |
| `workordercompletion` | `customform`, `isbackflush` |
| `workorderissue` | `customform` |
