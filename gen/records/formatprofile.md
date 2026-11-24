# Format Profile (`formatprofile`)
<!-- id: record:formatprofile -->

| Fact | Value |
|---|---|
| Internal ID | `formatprofile` |
| Category | list |
| Client scripts | yes |
| Server scripts | yes |
| Tags | `supports_create`, `supports_delete`, `supports_read`, `supports_update` |

## Record facts

- `requires_permission`: `financial_institution_records_full`
- `requires_feature`: `Expense Reporting` (when `expense_reporting_profile`) [`real_name`]
- `rule`: `one_financial_institution_per_profile`
- `rule`: `deleted_with_financial_institution`
- `cannot`: `delete_profile_with_imports`
- `sublists`: `account_linking`, `code_type_mapping`, `expense_code_mapping`
- `purpose`: `bank_and_card_data_import_for_reconciliation`, `card_charge_import_for_expense_reports`
- `ui_location`: `financial_institution_setup_add_format_profile`
- `available_only_when`: `bank_reconciliation_profile_type` [`accountmapping_sublist`, `trancodemapping_sublist`]
- `available_only_when`: `corporate_card_expenses_profile_type` [`expensecodemapping_sublist`]
- `not_supported`: `ui_only_subtabs_connectivity_parser_import_configuration`
- `caveat`: `import_configuration_subtab_holds_scriptable_fields` [`matchemployeeby`, `expensetype`, `expensesourcetype`]
- `ui_effect`: `import_configuration_subtab_replaces_account_linking` (when `corporate_card_expenses_profile_type`)
- `shown_only_when`: `parser_plugin_has_config_page_or_is_bank_statement_parser` [`parser_configuration_subtab`]
- `difference`: `financialinstitution_read_only_in_ui_but_settable_in_script`
- `ui_effect`: `financialinstitution_autofilled_in_ui`
- `use_instead`: `bank_feeds_suiteapp`, `amex_corporate_card_suiteapp` (when `creating_profiles_by_script`)

## Body fields

| Field ID | Type | Label | Required | submitField | Tags | Facts |
|---|---|---|---|---|---|---|
| `connectivitymethod` | select | Connectivity Method | no |  | marked_new | `purpose`: `bank_import_connectivity_method`; `default_value`: `Manual Import`; `requires`: `uploaded_connectivity_plugin` (when `automated_import`); `read_only_when`: `provisioned_by_bank_feeds_suiteapp` |
| `description` | text | Description | no |  |  | `purpose`: `format_profile_description`; `value_kind`: `free_text` |
| `expensesourcetype` | select | Employee Expense Source Type | no |  | marked_new | `purpose`: `expense_data_provider`; `options`: `American Express`, `Visa`, `Mastercard`, `imported_data`, `other` |
| `expensetype` | select | Expense Type | no |  | marked_new | `purpose`: `imported_charge_credit_target`; `options`: `Corporate Card`, `Reimbursable`; `option_meanings`: `corporate_card:company_pays_card_company`, `reimbursable:employee_pays_company_reimburses` |
| `financialinstitution` | select | Financial Institution | no |  |  | `purpose`: `financial_institution`; `value_source`: `parent_financial_institution` |
| `firstscheduledimportdate` | date | First Scheduled Import Date | no |  | marked_new | `purpose`: `first_scheduled_import_date`; `requires_together`: `firstscheduledimportdate`, `firstscheduledimporttime`, `timezone` |
| `firstscheduledimporttime` | timeofday | First Scheduled Import Time | no |  | marked_new | `purpose`: `first_scheduled_import_time`; `default_behavior`: `random_time_4_to_7_am_pacific` |
| `formatprofile` | text | Format Profile | yes |  |  | `purpose`: `format_profile_name` |
| `fprepeatevery` | select | Repeat Every | no |  | marked_new | `purpose`: `import_repeat_interval`; `constraint`: `positive_whole_number`; `requires_together`: `fprepeatevery`, `unitoftime`; `value_examples`: `6_hour_every_6_hours`, `1_day_daily` |
| `isinactive` | checkbox | Inactive | no |  |  | `purpose`: `inactivate_format_profile`; `cannot`: `import_with_inactive_profile`; `effect`: `scheduled_imports_suspended` |
| `lastsuccessfulimport` | datetime | Last Successful Import | no |  | marked_new | `purpose`: `last_successful_import`; `value_source`: `updated_after_each_successful_import` |
| `matchemployeeby` | select | Match Employee By | no |  | marked_new | `purpose`: `employee_match_method`; `options`: `cardholder_name`, `employee_id` |
| `notificationemail` | email | Notification Email | no |  | marked_new | `purpose`: `import_notification_email`; `effect`: `failures_always_notified_success_if_enabled`; `default_behavior`: `profile_creator_or_company_return_email` (when `blank`); `not_supported_for`: `manual_file_uploads` |
| `profiletype` | select | Profile Type | yes |  | marked_new | `purpose`: `format_profile_type`; `options`: `Bank Reconciliation`, `Corporate Card Expenses`; `default_value`: `Bank Reconciliation`; `cannot`: `change_after_set` |
| `sendsuccessnotification` | checkbox | Send Notification Email | no |  | marked_new | `purpose`: `send_success_notifications`; `not_supported_for`: `manual_file_uploads` |
| `timezone` | select | Time Zone | no |  | marked_new | `purpose`: `import_time_zone`; `effect`: `all_import_schedules_use_zone` |
| `transactionparser` | select | Transaction Parser | yes |  |  | `purpose`: `transaction_parser`; `requires`: `uploaded_parser_or_suiteapp_plugin`; `ui_effect`: `parser_configuration_subtab_for_configurable_plugins` |
| `unitoftime` | select | Unit of Time | no |  | marked_new | `purpose`: `import_interval_unit`; `allowed_values`: `hour`, `day` |
| `configurationid` | text | Configuration Id | no |  | marked_new |  |
| `createdby` | select | Created By | no |  |  |  |
| `createddate` | date | Created Date | no |  |  |  |
| `customform` | select | Custom Form | no |  |  |  |
| `externalid` | text | External ID | no |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |
| `lastmodifieddate` | date | Last Modified Date | no |  |  |  |
| `modifiedby` | select | Modified By | no |  |  |  |
| `templatestored` | checkbox |  | no |  |  |  |

## Sublist `accountmapping` (Account Mapping)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `accountmappingdescription` | text | Description | no |  |  |
| `accountmappingid` | integer | ID | no |  |  |
| `accountmappingkey` | text | Account Mapping Key | yes |  |  |
| `earliesttrandate` | date | Earliest Transaction Date for Import | no | marked_new |  |
| `formatprofile` | select | Format Profile | yes |  |  |
| `isaccountmappinginactive` | checkbox | Inactive | no |  |  |
| `mappednetsuiteaccount` | select | Mapped NetSuite Account | yes |  |  |

## Sublist `expensecodemapping` (Expense Code Mapping)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `description` | text | Description | no | marked_new |  |
| `expensecategory` | select | Expense Category | yes | marked_new |  |
| `expensecode` | text | Expense Code | yes | marked_new |  |
| `formatprofile` | select | Format Profile | yes | marked_new |  |
| `id` | integer | ID | no | marked_new |  |

## Sublist `trancodemapping` (Transaction Code Mapping)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `banktransactioncode` | text | Bank Transaction Code | yes |  |  |
| `creditdebit` | select | Credit / Debit | yes |  |  |
| `formatprofile` | select | Format Profile | yes |  |  |
| `istrancodemappingexclude` | checkbox | Exclude | no |  |  |
| `istrancodemappinginactive` | checkbox | Inactive | no |  |  |
| `netsuitetransactiontype` | select | NetSuite Transaction Type | yes |  |  |
| `trancodemappingdescription` | text | Description | no |  |  |
| `trancodemappingid` | integer | ID | no |  |  |
