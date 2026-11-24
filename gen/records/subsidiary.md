# Subsidiary (`subsidiary`)
<!-- id: record:subsidiary -->

| Fact | Value |
|---|---|
| Internal ID | `subsidiary` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `custom_fields_supported`, `scripting_full` |

## Record facts

- `script_types`: `client`, `server`
- `requires`: `oneworld`
- `rule`: `company_information_updates_sync_root_subsidiary`
- `script_functions`: `create`, `update`, `copy`, `delete`, `search`
- `triggered_by`: `company_information_page_update` [`subsidiary_user_events`]
- `side_effect`: `shared_field_update_syncs_company_information`
- `caveat`: `hidden_but_present_in_non_oneworld_accounts`
- `not_supported`: `standalone_user_event_in_non_oneworld` [`bundled_script_allowed`]
- `max_count`: `250` [`root_included`]
- `recommendation`: `avoid_commas_in_subsidiary_names`
- `reason`: `commas_delimit_multiselect_search_values`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `checklayout` | select | Check Default Chart Type | yes | no |  |  | `purpose`: `check_layout` |
| `country` | select | Country | yes | no |  |  | `purpose`: `country` |
| `currency` | select | Currency | yes | no |  |  | `purpose`: `base_currency`; `default_behavior`: `parent_subsidiary_currency` (when `blank`); `editable_only_when`: `no_transactions_entered` |
| `edition` | text | Edition | no | no |  |  | `purpose`: `netsuite_edition`; `value_source`: `system_displayed` |
| `email` | text | Return Email Address | no | no |  |  | `purpose`: `return_email_address` |
| `fax` | text | Fax | no | no |  |  | `purpose`: `fax_number` |
| `federalidnumber` | text | Employer Identification Number (EIN) | no | no |  |  | `purpose`: `tax_id_number`; `option_meanings`: `us:ein_or_fin`, `australia:abn`, `uk:vat_number`, `other:vat_number`; `required_when`: `employees_or_corporation_or_partnership` |
| `fiscalcalendar` | select | Fiscal Calendar | yes | no |  |  | `purpose`: `fiscal_calendar`; `effect`: `sets_accounting_year_start` |
| `intercoaccount` | select | Intercompany Account | no | no |  |  | `purpose`: `intercompany_account`; `requires_feature`: `Intercompany Time and Expense` [`real_name`]; `value_source`: `auto_associated_by_base_currency` |
| `iselimination` | checkbox | Elimination | no | no |  |  | `purpose`: `elimination_subsidiary`; `true_means`: `only_intercompany_entries` |
| `isinactive` | checkbox | Subsidiary is Inactive | no | no |  |  | `purpose`: `inactivate_subsidiary`; `ui_effect`: `hidden_unless_already_attached`; `effect`: `subsidiary_web_sites_offline` |
| `languagelocale` | select | Language | yes | no |  |  | `purpose`: `default_user_language`; `can`: `users_override_in_preferences` |
| `legalname` | text | Legal Name | no | no |  |  | `purpose`: `legal_name`; `displayed_on`: `tax_forms` |
| `logo` | select | Subsidiary Logo (Forms) | no | no |  |  | `purpose`: `forms_logo`; `displayed_on`: `all_forms`; `not_displayed_on`: `account_pages`; `allowed_values`: `JPG`, `GIF`; `requires`: `image_available_without_login`; `ui_effect`: `new_option_creates_record` |
| `mossapplies` | checkbox | MOSS applies | no | no |  |  | `purpose`: `oss_vat_applies` |
| `name` | text | Name | yes | no |  |  | `purpose`: `subsidiary_name`; `displayed_on`: `forms`; `not_displayed_on`: `checks`; `format`: `alphanumeric`; `max_length`: `83` characters |
| `pagelogo` | select | Subsidiary Logo (Pages) | no | no |  |  | `purpose`: `pages_logo`; `displayed_on`: `account_pages`; `not_displayed_on`: `printed_forms`; `allowed_values`: `JPG`, `GIF`; `requires`: `image_available_without_login`; `ui_effect`: `new_option_creates_record` |
| `parent` | select | Subsubsidiary of | yes | no |  |  | `purpose`: `parent_subsidiary`; `effect`: `sets_company_hierarchy` |
| `purchaseorderamount` | posfloat | Vendor Bill - Purchase Order Amount Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_po_amount` |
| `purchaseorderquantity` | posfloat | Vendor Bill - Purchase Order Quantity Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_po_quantity` |
| `purchaseorderquantitydiff` | posfloat | Vendor Bill - Purchase Order Quantity Difference | no | no |  |  | `purpose`: `tolerance_bill_vs_po_quantity_difference` |
| `receiptamount` | posfloat | Vendor Bill - Item Receipt Amount Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_amount` |
| `receiptquantity` | posfloat | Vendor Bill - Item Receipt Quantity Tolerance | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_quantity` |
| `receiptquantitydiff` | posfloat | Vendor Bill - Item Receipt Quantity Difference | no | no |  |  | `purpose`: `tolerance_bill_vs_receipt_quantity_difference` |
| `roundtime` | select | Round Time Entry Duration | no | no | yes |  | `purpose`: `round_time_entries`; `value_source`: `round_time_entry_duration_preference` |
| `showsubsidiaryname` | checkbox | Always Display Subsidiary Name | no | no |  |  | `purpose`: `show_only_subsidiary_name`; `ui_effect`: `center_dashboards_show_subsidiary_logo`; `false_means`: `parent_and_subsidiary_names_shown` |
| `ssnortin` | text | SSN or TIN (Social Security Number, Tax ID Number) | no | no |  |  | `purpose`: `ssn_or_tin`; `applies_when`: `self_employed_without_employees` |
| `state` | text | State/Province | no | no |  |  | `purpose`: `state_or_province` |
| `taxfiscalcalendar` | select | Tax Fiscal Calendar | yes | no |  |  | `purpose`: `tax_fiscal_calendar`; `effect`: `sets_tax_year_start` |
| `tranprefix` | text | Transaction Prefix | no | no |  |  | `purpose`: `transaction_number_prefix`; `max_length`: `8` characters; `format`: `alphanumeric`; `configured_by`: `auto_generated_numbers_use_subsidiary` |
| `url` | text | Web Site | no | no |  |  | `purpose`: `subsidiary_website` |
| `addrphone` | phone | Phone | no | no |  |  |  |
| `addrtext` | address | Address | no | no |  |  |  |
| `attention` | text | Attention | no | no |  |  |  |
| `caseupdatetemplate` | select | Case Update | no | no |  |  |  |
| `city` | text | City | no | no |  |  |  |
| `externalid` | text | ExternalId | no | no |  |  |  |
| `heading0` | text | Case Email Setup | no | no |  |  |  |
| `heading1` | text | Customer Notification Templates | no | no |  |  |  |
| `heading2` | text | Employee Notification Templates | no | no |  |  |  |
| `mainaddress` | summary |  | no | no |  |  |  |
| `override` | checkbox | Override | no | no |  |  |  |
| `returnaddr` | address | Address | no | no |  |  |  |
| `returnaddress` | summary |  | no | no |  |  |  |
| `returnaddress1` | text | Address1 | no | no |  |  |  |
| `returnaddress2` | text | Address2 | no | no |  |  |  |
| `returncity` | text | City | no | no |  |  |  |
| `returncountry` | select | Country | no | no |  |  |  |
| `returnstate` | text | County/State/Province | no | no |  |  |  |
| `returnzip` | text | Postal Code | no | no |  |  |  |
| `shipaddr` | address | Address | no | no |  |  |  |
| `shipcountry` | select | Country | no | no |  |  |  |
| `shippingaddress` | summary |  | no | no |  |  |  |
| `zip` | text | Zip | no | no |  |  |  |

## Sublist `accountingbookdetail` (Currencies)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountingbook` | select | Secondary Book | no |  |  |
| `currency` | select | Base Currency | yes |  |  |

## Sublist `taxregistration` (Tax Registrations)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `effectivefrom` | date | Effective From | yes |  |  |
| `nexus` | select | Nexus | yes |  |  |
| `nexuscountry` | select | Country | yes |  |  |
| `taxagency` | select | Tax Agency | no |  |  |
| `taxengine` | select | Tax Engine | no |  |  |
| `taxregistrationnumber` | text | Tax Reg. Number | no |  |  |
| `validuntil` | date | Valid Until | no |  |  |

## Tabs

| Tab ID | Label |
|---|---|
| `addressbook` | Address |
| `history` | System Notes |
| `nexustab` | Nexuses |
| `preferencestab` | Preferences |
| `returnsaddrtab` | Return Address |
| `shippingaddrtab` | Shipping Address |
| `translation` | Translation |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `accountingbookcurrency` | select | Accounting Book Currency |  |
| `address` | textarea | Address |  |
| `city` | text | City |  |
| `country` | select | Country |  |
| `currency` | select | Currency |  |
| `effectivefrom` | date | Effective From |  |
| `email` | email | Email |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `fax` | phone | Fax |  |
| `formuladate` | date | Formula (Date) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `iselimination` | checkbox | Elimination |  |
| `isinactive` | checkbox | Inactive |  |
| `legalname` | text | Legal Name |  |
| `mossapplies` | checkbox | MOSS Applies |  |
| `name` | text | Name |  |
| `nexus` | select | Nexus |  |
| `phone` | phone | Phone |  |
| `purchaseorderamount` | float | Vendor Bill - Purchase Order Amount Tolerance |  |
| `purchaseorderquantity` | float | Vendor Bill - Purchase Order Quantity Tolerance |  |
| `purchaseorderquantitydiff` | float | Vendor Bill - Purchase Order Quantity Difference |  |
| `receiptamount` | float | Vendor Bill - Item Receipt Amount Tolerance |  |
| `receiptquantity` | float | Vendor Bill - Item Receipt Quantity Tolerance |  |
| `receiptquantitydiff` | float | Vendor Bill - Item Receipt Quantity Difference |  |
| `state` | select | State/Province |  |
| `taxengine` | select | Tax Engine |  |
| `taxidnum` | text | Tax ID |  |
| `taxregistrationnumber` | text | Tax Reg. Number |  |
| `tranprefix` | text | Transaction Prefix |  |
| `url` | url | Web Site |  |
| `validuntil` | date | Valid Until |  |
| `zip` | text | Zip |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `accountingbook` | select | Accounting Book |  |
| `accountingbookcurrency` | select | Accounting Book Currency |  |
| `anonymouscustomerinboundemail` | text | Anonymous Customer - Inbound Email |  |
| `anonymouscustomeronlineforms` | text | Anonymous Customer - Online Forms |  |
| `caseassignmenttemplate` | text | Employee Notification Template - Case Assignment |  |
| `caseautomaticclosuretemplate` | text | Customer Notification Template - Automatic Case Closure |  |
| `casecopyemployeetemplate` | text | Employee Notification Template - Copy Employee |  |
| `casecreationtemplate` | text | Customer Notification Template - Case Creation |  |
| `caseescalationtemplate` | text | Employee Notification Template - Escalation |  |
| `caseupdatetemplate` | text | Customer Notification Template - Case Update |  |
| `city` | text | City |  |
| `companynameforsupportmessages` | text | Company Name for Support Messages |  |
| `country` | select | Country |  |
| `currency` | select | Currency |  |
| `effectivefrom` | date | Effective From |  |
| `email` | email | Email |  |
| `employeecaseupdatetemplate` | text | Employee Notification Template - Case Update |  |
| `externalid` | select | External ID |  |
| `fax` | phone | Fax |  |
| `formulacurrency` | currency | Formula (Currency) |  |
| `formuladate` | date | Formula (Date) |  |
| `formuladatetime` | datetime | Formula (Date/Time) |  |
| `formulanumeric` | float | Formula (Numeric) |  |
| `formulapercent` | percent | Formula (Percent) |  |
| `formulatext` | text | Formula (Text) |  |
| `internalid` | select | Internal ID |  |
| `iselimination` | checkbox | Elimination |  |
| `isinactive` | checkbox | Inactive |  |
| `legalname` | text | Legal Name |  |
| `mainsupportemailaddress` | email | Main Support Email Address |  |
| `mossapplies` | checkbox | MOSS Applies |  |
| `name` | text | Name |  |
| `namenohierarchy` | text | Name (no hierarchy) |  |
| `nexus` | select | Nexus |  |
| `phone` | phone | Phone |  |
| `purchaseorderamount` | float | Vendor Bill - Purchase Order Amount Tolerance |  |
| `purchaseorderquantity` | float | Vendor Bill - Purchase Order Quantity Tolerance |  |
| `purchaseorderquantitydiff` | float | Vendor Bill - Purchase Order Quantity Difference |  |
| `receiptamount` | float | Vendor Bill - Item Receipt Amount Tolerance |  |
| `receiptquantity` | float | Vendor Bill - Item Receipt Quantity Tolerance |  |
| `receiptquantitydiff` | float | Vendor Bill - Item Receipt Quantity Difference |  |
| `state` | select | State/Province |  |
| `taxengine` | select | Tax Engine |  |
| `taxidnum` | text | Tax ID |  |
| `taxregistrationnumber` | text | Tax Reg. Number |  |
| `tranprefix` | text | Transaction Prefix |  |
| `url` | url | Web Site |  |
| `validuntil` | date | Valid Until |  |
| `zip` | text | Zip |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `address` | Address | Address |
| `defaultadvancetoapplyaccount` | Default Advance to Apply Account | Account |
| `returnaddress` | Return Address | Address |
| `shippingaddress` | Shipping Address | Address |
