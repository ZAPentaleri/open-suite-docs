# Opportunity (`opportunity`)
<!-- id: record:opportunity -->

| Fact | Value |
|---|---|
| Internal ID | `opportunity` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `initialization_default_value`: `entity`, `entity_internal_id` (refs `N/record.create`, `N/record.copy`, `N/record.transform`, `N/record.load`)
- `script_types`: `client`, `server`
- `script_functions`: `create`, `copy`, `update`, `delete`, `search`, `transform`
- `body_only_scriptable_fields`: `estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`
- `requires_feature`: `Opportunities` [`real_name`]
- `doc_inconsistency`: `functions_sentence_names_manufacturing_operation_task`
- `purpose`: `negotiation_with_prospect`
- `configured_by`: `enable_features_crm_subtab_sales` [`opportunities`]
- `sublist_requires_feature`: `partners:multi_partner_management`
- `caveat`: `gross_profit_fields_not_scriptable_at_line_level`
- `supported_in`: `transaction_search` [`estgrossprofit`, `estgrossprofitpercent`, `totalcostestimate`]

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `actionitem` | text | Action Item | no | yes |  |  | `purpose`: `next_negotiation_step` |
| `altsalesrangehigh` | currency | To | no | no |  |  | `purpose`: `alternate_sales_range`; `constraint`: `low_at_most_altsalestotal_high_at_least_altsalestotal`; `option_meanings`: `low:worst_case_total`, `high:upside_total`; `help_text_shared_with`: `opportunity.altsalesrangelow` |
| `altsalesrangelow` | currency | Range | no | no |  |  | `purpose`: `alternate_sales_range`; `constraint`: `low_at_most_altsalestotal_high_at_least_altsalestotal`; `option_meanings`: `low:worst_case_total`, `high:upside_total`; `help_text_shared_with`: `opportunity.altsalesrangehigh` |
| `billaddress` | address | Bill To | no | no |  |  | `purpose`: `billing_address`; `default_behavior`: `customer_default_billing_address`; `can`: `choose_other_or_new_address_in_selector` |
| `billaddresslist` | select | Bill To Select | no | no |  |  | `purpose`: `billing_address_selector`; `ui_effect`: `new_option_creates_record` |
| `buyingreason` | select | Buying Reason | no | yes |  |  | `purpose`: `buying_reason`; `allowed_values_listed_in`: `crm_list_buying_reason` |
| `buyingtimeframe` | select | Buying Time Frame | no | yes |  |  | `purpose`: `buying_time_frame`; `allowed_values_listed_in`: `crm_list_buying_time_frame` |
| `class` | select | Class | no | no |  |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `opportunity.item.class` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `transaction_currency`; `default_behavior`: `customer_primary_currency`; `requires_feature`: `Multi-Currency Customers` (when `choosing_transaction_currency`) [`real_name`]; `ui_effect`: `reprices_items_and_recalculates_tax_and_profit`; `caveat`: `custom_prices_not_repriced` |
| `currencyname` | text | Currency | no | no |  |  | `purpose`: `currency_name`; `value_source`: `system_displayed` |
| `customform` | select | Custom Form | yes | no |  |  | `purpose`: `custom_form`; `ui_effect`: `new_option_creates_record` |
| `department` | select | Department | no | no |  |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `opportunity.item.department` |
| `entity` | select | Company | yes | no |  |  | `purpose`: `opportunity_company`; `ui_effect`: `new_option_creates_record` |
| `entitystatus` | select | Status | yes | yes |  |  | `purpose`: `opportunity_status`; `effect`: `closed_won_when_other_transaction_linked`; `effect`: `follows_linked_estimate_or_order_status` |
| `entitytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `customer_tax_registration` |
| `estgrossprofit` | currency | Est. Gross Profit | no | no | yes |  | `purpose`: `estimated_gross_profit`; `value_source`: `revenue_minus_estimated_cost` |
| `estgrossprofitpercent` | percent | Est. Gross Profit Percent | no | no | yes |  | `purpose`: `estimated_gross_margin_percent`; `value_source`: `estimated_profit_divided_by_revenue` |
| `estimatedbudget` | currency | Estimated Budget | no | yes |  |  | `purpose`: `estimated_budget` |
| `exchangerate` | currency2 | Exchange Rate | yes | no |  |  | `purpose`: `exchange_rate`; `can`: `override_per_transaction` |
| `expectedclosedate` | date | Expected Close | yes | yes |  |  | `purpose`: `expected_close_date` |
| `forecasttype` | select | Forecast Type | no | yes |  |  | `purpose`: `forecast_category`; `special_values`: `Omitted` |
| `isbudgetapproved` | checkbox | Budget Approved | no | yes |  |  | `purpose`: `budget_approved` |
| `job` | select | Project | no | no |  |  | `purpose`: `project` |
| `leadsource` | select | Lead Source | no | no |  |  | `purpose`: `lead_source`; `allowed_values_listed_in`: `campaigns` (when `marketing_automation`); `allowed_values_listed_in`: `crm_list_lead_source` (when `no_marketing_automation`) |
| `location` | select | Location | no | no |  |  | `purpose`: `location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `opportunity.item.location` |
| `memo` | textarea | Details | no | no |  |  | `purpose`: `memo` |
| `nexus` | select | Nexus | no | no |  |  | `purpose`: `transaction_nexus`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `partner` | select | Partner | no | yes |  |  | `purpose`: `partner`; `allowed_values_listed_in`: `partner_records`; `ui_effect`: `new_option_creates_record` |
| `probability` | percent | Probability | yes | yes |  |  | `purpose`: `win_probability`; `value_source`: `linked_estimate_probability` (when `estimate_linked`) |
| `projaltsalesamt` | currency | Alt. Sales Total | no | yes |  |  | `purpose`: `projected_alternate_sales`; `value_source`: `items_and_linked_estimates` |
| `projectedtotal` | currency | Projected Total | yes | yes |  |  | `purpose`: `projected_total`; `value_source`: `items_and_linked_estimates` |
| `rangehigh` | currency | To | no | yes |  |  | `purpose`: `amount_range`; `constraint`: `low_at_most_projectedtotal_high_at_least_projectedtotal`; `option_meanings`: `low:worst_case_total`, `high:upside_total`; `help_text_shared_with`: `opportunity.rangelow` |
| `rangelow` | currency | Range | no | yes |  |  | `purpose`: `amount_range`; `constraint`: `low_at_most_projectedtotal_high_at_least_projectedtotal`; `option_meanings`: `low:worst_case_total`, `high:upside_total`; `help_text_shared_with`: `opportunity.rangehigh` |
| `salesgroup` | select | Choose Team | no | no |  |  | `purpose`: `sales_team`; `allowed_values_listed_in`: `sales_team_groups` |
| `salesreadiness` | select | Sales Readiness | no | yes |  |  | `purpose`: `sales_readiness`; `allowed_values_listed_in`: `crm_list_sales_readiness` |
| `salesrep` | select | Sales Rep | no | yes |  |  | `purpose`: `sales_rep`; `default_behavior`: `customer_sales_rep`; `allowed_values_listed_in`: `employee_sales_reps`; `can`: `override_per_transaction` |
| `shipaddress` | address | Ship To | no | no |  |  | `purpose`: `shipping_address`; `default_behavior`: `customer_default_shipping_address`; `doc_inconsistency`: `unresolved_navigation_placeholders` |
| `shipaddresslist` | select | Ship To Select | no | no |  |  | `purpose`: `shipping_address_selector`; `ui_effect`: `new_option_creates_record` |
| `subsidiary` | select | Subsidiary | no | no |  |  | `purpose`: `subsidiary`; `value_source`: `system_displayed` |
| `subsidiarytaxregnum` | select | Tax Reg. Number | no |  |  |  | `purpose`: `subsidiary_tax_registration`; `value_source`: `nexus_lookup`; `can`: `override_with_nexus_override` |
| `syncpartnerteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_partner_team_to_customer` |
| `syncsalesteams` | checkbox | Update Customer | no | no |  |  | `purpose`: `sync_sales_team_to_customer` |
| `taxdetailsoverride` | checkbox | Tax Details Override | no |  |  |  | `purpose`: `override_tax_details`; `requires_permission`: `tax_details_tab_edit` |
| `taxpointdate` | date | Tax Point Date | no | no |  |  | `purpose`: `tax_point_date`; `value_kind`: `date_determining_taxability` |
| `taxregoverride` | checkbox | Tax Registration Override | no |  |  |  | `purpose`: `override_nexus_and_tax_registration`; `requires_permission`: `tax_details_tab_edit` |
| `title` | text | Title | no | yes |  |  | `purpose`: `opportunity_title`; `displayed_on`: `lists`, `reports`, `opportunities_subtab` |
| `totalcostestimate` | currency | Est. Extended Cost | no | no |  |  | `purpose`: `estimated_cost`; `value_source`: `estimated_rate_times_quantity` |
| `trandate` | date | Date Created | yes | no |  |  | `purpose`: `opportunity_date`; `default_behavior`: `creation_date`; `can`: `override` |
| `tranid` | text | Opportunity # | no | no |  |  | `purpose`: `opportunity_number`; `value_source`: `system_generated`; `can`: `override` |
| `weightedtotal` | currency | Weighted Total | no | no |  |  | `purpose`: `weighted_total`; `value_source`: `projected_total_times_probability` (when `zero_or_one_transaction`); `value_source`: `sum_of_transaction_weighted_totals` (when `several_transactions`); `constraint`: `only_estimates_included_in_forecast_count` |
| `winlossreason` | select | Win/Loss Reason | no | yes |  |  | `purpose`: `win_loss_reason`; `allowed_values_listed_in`: `crm_list_win_loss_reason` |
| `altsalestotal` | text | Total (Alt. Sales Amount) | no | no |  |  |  |
| `balance` | currency | Balance | no | no |  |  |  |
| `billingaddress` | summary |  | no | no |  |  |  |
| `billisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `consolidatebalance` | currency | Consolidated Balance | no | no |  |  |  |
| `createddate` | datetime |  | no | no |  |  |  |
| `currencysymbol` | text | Currency Symbol | no | no |  |  |  |
| `documentstatus` | text |  | no | no |  |  |  |
| `entitynexus` | select | Nexus | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `isbasecurrency` | checkbox | Base Currency | no | no |  |  |  |
| `lastmodifieddate` | datetime |  | no | no |  |  |  |
| `onetime` | currency2 | One Time | no | no |  |  |  |
| `recurannually` | currency2 | Annually | no | no |  |  |  |
| `recurmonthly` | currency2 | Monthly | no | no |  |  |  |
| `recurquarterly` | currency2 | Quarterly | no | no |  |  |  |
| `recurweekly` | currency2 | Weekly | no | no |  |  |  |
| `shipisresidential` | text | Shipping address is residential | no | no |  |  |  |
| `shipoverride` | text |  | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `source` | text | Source | no | no |  |  |  |
| `status` | text |  | no | no |  |  |  |
| `statusref` | text |  | no | no |  |  |  |
| `taxpointdateoverride` | checkbox | Override | no | no |  |  |  |
| `total` | text |  | no | no |  |  |  |
| `unbilledorders` | currency | Unbilled Orders | no | no |  |  |  |

## Sublist `item` (Items)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `class` | select | Class | no |  | `purpose`: `class`; `allowed_values_listed_in`: `class_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `opportunity.class` |
| `department` | select | Department | no |  | `purpose`: `department`; `allowed_values_listed_in`: `department_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `opportunity.department` |
| `location` | select | Location | no |  | `purpose`: `location`; `allowed_values_listed_in`: `location_records`; `ui_effect`: `new_option_creates_record`; `help_text_shared_with`: `opportunity.location` |
| `altsalesamt` | currency | Alt. Sales Amount | no |  |  |
| `amount` | currency | Amount | no |  |  |
| `billingschedule` | text |  | no |  |  |
| `billvariancestatus` | text |  | no |  |  |
| `catchupperiod` | select |  | no |  |  |
| `costestimate` | currency | Est. Extended Cost | no |  |  |
| `costestimaterate` | rate |  | no |  |  |
| `costestimatetype` | select | Cost Estimate Type | no |  |  |
| `daysbeforeexpiration` | text |  | no |  |  |
| `deferrevrec` | checkbox |  | no |  |  |
| `description` | textarea | Description | no |  |  |
| `expectedshipdate` | date | Expected Ship Date | no |  |  |
| `fromjob` | checkbox | Project Item | no |  |  |
| `grossamt` | currency | Gross Amt | no |  |  |
| `id` | text |  | no |  |  |
| `isestimate` | checkbox | Billable Estimate | no |  |  |
| `istaxable` | text |  | no |  |  |
| `isvsoebundle` | text |  | no |  |  |
| `item` | select | Item | yes |  |  |
| `itemsubtype` | text |  | no |  |  |
| `itemtype` | text |  | no |  |  |
| `job` | select | Job | no |  |  |
| `line` | text |  | no |  |  |
| `linenumber` | integer |  | no |  |  |
| `matrixtype` | text |  | no |  |  |
| `options` | namevaluelist | Options | no |  |  |
| `price` | select | Price Level | no |  |  |
| `printitems` | text |  | no |  |  |
| `quantity` | float | Quantity | no |  |  |
| `rate` | rate | Rate | no |  |  |
| `rateschedule` | text |  | no |  |  |
| `subscription` | select | Subscription | no |  |  |
| `tax1amt` | currency | Tax Amt | no |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxrate1` | percent | Tax Rate | no |  |  |
| `units` | select | Units | no |  |  |

## Sublist `accountingbookdetail` (Accounting Books)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `exchangerate` | currency2 | Exchange Rate | yes |  |  |

## Sublist `competitors` (Competitors)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `competitor` | select | Name | yes |  |  |
| `id` | text |  | no |  |  |
| `notes` | textarea | Notes | no |  |  |
| `url` | url | URL | no |  |  |
| `winner` | checkbox | Winner | no |  |  |

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

## Sublist `taxdetails` (Tax Detail)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `calcdetail` | textarea | Details | no |  |  |
| `linename` | text | Name | no |  |  |
| `linetype` | text | Line type | no |  |  |
| `netamount` | currency | Net Amount | no |  |  |
| `taxamount` | currency | Tax Amount | yes |  |  |
| `taxbasis` | currency | Tax Basis | yes |  |  |
| `taxcode` | select | Tax Code | yes |  |  |
| `taxdetailsreference` | text | Tax Details Reference | no |  |  |
| `taxrate` | percent | Tax Rate | yes |  |  |
| `transaction` | select | Transaction | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `address` | Address |
| `custom` | Custom |
| `general` | General |
| `items` | Items |
| `multipartner` | Partners |
| `qualification` | Qualification |
| `teamselling` | Sales Team |
| `workflow` | Workflow |

## Transform targets

| Target record | Default fields |
|---|---|
| `cashsale` | `customform`, `billdate` |
| `estimate` | `customform` |
| `invoice` | `customform`, `billdate` |
| `salesorder` | `customform` |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `amount` | currency | Item Amount |  |
| `availableoffline` | checkbox | Always Available Offline |  |
| `buyingreason` | select | Buying Reason |  |
| `buyingtimeframe` | select | Buying Time Frame |  |
| `class` | select | Class |  |
| `closedate` | date | Actual Close |  |
| `competitor` | select | Competitor |  |
| `contribution` | integer | Contribution % |  |
| `currency` | select | Currency |  |
| `custtype` | select | Customer Category |  |
| `datecreated` | datetime | Date Created |  |
| `daysopen` | integer | Days Open |  |
| `daystoclose` | integer | Days To Close |  |
| `department` | select | Department |  |
| `entity` | select | Prospect/Customer |  |
| `entitystatus` | select | Opportunity Status |  |
| `estimatedbudget` | currency | Estimated Budget |  |
| `expectedclosedate` | date | Expected Close Date |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `forecasttype` | select | Forecast Type |  |
| `foreignprojectedamount` | currency | Projected Total (Foreign Currency) |  |
| `foreignrangehigh` | currency | Range - High (Foreign Currency) |  |
| `foreignrangelow` | currency | Range - Low (Foreign Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxtrancostestimate` | currency | Est. Extended Cost (Transaction) (Foreign Currency) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isbudgetapproved` | checkbox | Budget Approved |  |
| `item` | multiselect | Item |  |
| `lastmodifieddate` | datetime | Date Last Modified |  |
| `leadsource` | select | Lead Source |  |
| `location` | select | Location |  |
| `memo` | text | Memo |  |
| `number` | integer | Number |  |
| `partner` | select | Partner |  |
| `partnercontribution` | integer | Partner Contribution % |  |
| `partnerrole` | select | Partner Role |  |
| `partnerteammember` | select | Partner Team Member |  |
| `postingperiod` | period | Period |  |
| `probability` | integer | Probability |  |
| `projaltsalesamt` | currency | Projected Alt.Sales Amount |  |
| `projectedtotal` | currency | Projected Amount |  |
| `rangehigh` | currency | Range - High |  |
| `rangehighalt` | currency | Alt.Sales Range - High |  |
| `rangelow` | currency | Range - Low |  |
| `rangelowalt` | currency | Alt.Sales Range - Low |  |
| `salesreadiness` | select | Sales Readiness |  |
| `salesrep` | select | Sales Rep |  |
| `salesteammember` | select | Sales Team Member |  |
| `salesteamrole` | select | Sales Team Role |  |
| `status` | select | Document Status |  |
| `subsidiary` | select | Subsidiary |  |
| `taxperiod` | period | Tax Period |  |
| `title` | text | Title |  |
| `trancostestimate` | currency | Est. Extended Cost (Transaction) |  |
| `trandate` | date | Date |  |
| `tranestgrossprofit` | currency | Est. Gross Profit (Transaction) |  |
| `tranestgrossprofitpct` | percent | Est. Gross Profit Percent (Transaction) |  |
| `tranfxestgrossprofit` | currency | Est. Gross Profit (Transaction) (Foreign Currency) |  |
| `tranid` | text | Number/ID |  |
| `winlossreason` | select | Win/Loss Reason |  |
| `wonby` | select | Won By |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `actionitem` | text | Action Item |  |
| `availableoffline` | checkbox | Always Available Offline |  |
| `buyingreason` | text | Buying Reason |  |
| `buyingtimeframe` | text | Buying Time Frame |  |
| `class` | select | Class |  |
| `classnohierarchy` | select | Class (no hierarchy) |  |
| `closedate` | date | Actual Close |  |
| `competitor` | select | Competitor |  |
| `contribution` | percent | Contribution % |  |
| `contributionprimary` | percent | Primary Sales Rep Contribution % |  |
| `currency` | select | Currency |  |
| `custtype` | text | Customer Category |  |
| `datecreated` | datetime | Date Created |  |
| `daysopen` | integer | Days Open |  |
| `daystoclose` | integer | Days To Close |  |
| `decisionmaker` | select | Decision Maker |  |
| `department` | select | Department |  |
| `departmentnohierarchy` | select | Department (no hierarchy) |  |
| `documentstatus` | select | Document Status |  |
| `entity` | select | Customer |  |
| `entitystatus` | select | Opportunity Status |  |
| `estimatedbudget` | currency | Estimated Budget |  |
| `expectedclosedate` | date | Expected Close |  |
| `externalid` | select | External ID |  |
| `forecasttype` | select | Forecast Type |  |
| `foreignprojectedamount` | currency | Projected Total (Foreign Currency) |  |
| `foreignrangehigh` | currency | Range - High (Foreign Currency) |  |
| `foreignrangelow` | currency | Range - Low (Foreign Currency) |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `fxtrancostestimate` | currency | Est. Extended Cost (Transaction) (Foreign Currency) |  |
| `internalid` | select | Internal ID |  |
| `isbudgetapproved` | checkbox | Budget Approved |  |
| `lastmodifieddate` | datetime | Last Modified |  |
| `lastviewed` | datetime | Last Viewed |  |
| `leadsource` | select | Lead Source |  |
| `location` | select | Location |  |
| `locationnohierarchy` | select | Location (no hierarchy) |  |
| `memo` | text | Memo |  |
| `partner` | select | Partner |  |
| `partnercontribution` | percent | Partner Contribution % |  |
| `partnerrole` | select | Partner Role |  |
| `partnerteammember` | select | Partner Team Member |  |
| `period` | text | Period |  |
| `probability` | percent | Probability |  |
| `projaltsalesamt` | currency | Projected Alt.Sales Total |  |
| `projectedtotal` | currency | Projected Total |  |
| `rangehigh` | currency | Range - High |  |
| `rangehighalt` | currency | Alt.Sales Range - High |  |
| `rangelow` | currency | Range - Low |  |
| `rangelowalt` | currency | Alt.Sales Range - Low |  |
| `salesreadiness` | text | Sales Readiness |  |
| `salesrep` | select | Sales Rep |  |
| `salesteammember` | select | Sales Team Member |  |
| `salesteamrole` | select | Sales Team Role |  |
| `subsidiary` | select | Subsidiary |  |
| `subsidiarynohierarchy` | select | Subsidiary (no hierarchy) |  |
| `taxperiod` | select | Tax Period |  |
| `title` | text | Title |  |
| `total` | currency | Item Amount |  |
| `trancostestimate` | currency | Est. Extended Cost (Transaction) |  |
| `trandate` | date | Date |  |
| `tranestgrossprofit` | currency | Est. Gross Profit (Transaction) |  |
| `tranestgrossprofitpct` | percent | Est. Gross Profit Percent (Transaction) |  |
| `tranfxestgrossprofit` | currency | Est. Gross Profit (Transaction) (Foreign Currency) |  |
| `tranid` | text | Number |  |
| `weightedtotal` | currency | Weighted Total |  |
| `winlossreason` | select | Win/Loss Reason |  |
| `wonby` | select | Won By |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `competitor` | Competitor | Competitor |
| `competitorwinner` | Competitor (Winner) | Competitor |
| `contact` | Contact | Contact |
| `contactprimary` | Contact (Primary) | Contact |
| `customer` | Customer | Customer |
| `decisionmaker` | Decision Maker | Contact |
| `leadsource` | Lead Source | Campaign |
| `messages` | Messages | Message |
| `originatinglead` | Originating Lead | Lead |
| `partner` | Partner | Partner |
| `salesrep` | Sales Rep | Employee |
| `task` | Task | Task |

## Suspected source errors

> [!SOURCE-ERROR] `functions_sentence_names_manufacturing_operation_task`
<!-- source-error: record:opportunity | - | functions_sentence_names_manufacturing_operation_task -->

> [!SOURCE-ERROR] `unresolved_navigation_placeholders` (on `shipaddress`)
<!-- source-error: record:opportunity | shipaddress | unresolved_navigation_placeholders -->
