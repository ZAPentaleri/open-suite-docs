# Catalog: csv_import_sublists
<!-- id: catalog:csv_import_sublists -->

Used with: `N/task.CsvImportTask`, `ss1:nlobjCSVImport#setLinkedFile`

| ID | Label | Group | Facts |
|---|---|---|---|
| `accounting_chart_of_accounts:translations` | Translations | Accounting: Chart of Accounts | `requires_feature`: `Multi-Language` [`real_name`]; `selective_update_key`: `Language` [`real_name`] |
| `accounting_expense_categories:rates` | Rates | Accounting: Expense Categories | `available_only_in`: `OneWorld` [`real_name`]; `selective_update_key`: `Subsidiary` [`real_name`] |
| `accounting_price_book:price_book_lines` | Price Book Lines | Accounting: Price Book | `requires_feature`: `SuiteBilling` [`real_name`]; `selective_update_key`: `Subscription Plan Line` [`real_name`] |
| `accounting_price_plan:pricing` | Pricing | Accounting: Price Plan | `requires_feature`: `SuiteBilling` [`real_name`]; `selective_update_key`: `Above Quantity` [`real_name`] |
| `activities_events:attendees` | Attendees | Activities: Events | `selective_update_key`: `Attendee` [`real_name`] |
| `activities_events:resources` | Resources | Activities: Events | `selective_update_key`: `Resource` [`real_name`] |
| `activities_events:time_track` | Time Track | Activities: Events |  |
| `activities_phone_calls:contacts` | Contacts | Activities: Phone Calls | `selective_update_key`: `Participant`, `Contact` [`real_name`] |
| `activities_phone_calls:time_tracking` | Time Tracking | Activities: Phone Calls | `requires_feature`: `Time Tracking` [`real_name`] |
| `activities_tasks:contacts` | Contacts | Activities: Tasks | `selective_update_key`: `Company/Project`, `Contact` [`real_name`] |
| `activities_tasks:time_tracking` | Time Tracking | Activities: Tasks | `requires_feature`: `Time Tracking` [`real_name`] |
| `classification_class:class_translations` | Class Translations | Classification: Class | `requires_feature`: `Multi-Language` [`real_name`]; `selective_update_key`: `Language` [`real_name`] |
| `classification_department:department_translations` | Department Translations | Classification: Department | `requires_feature`: `Multi-Language` [`real_name`]; `selective_update_key`: `Language` [`real_name`] |
| `classification_location:location_translations` | Location Translations | Classification: Location | `requires_feature`: `Multi-Language` [`real_name`]; `selective_update_key`: `Language` [`real_name`] |
| `classification_merchandise_hierarchy_node:hierarchy_versions` | Hierarchy Versions | Classification: Merchandise Hierarchy Node | `ui_field`: `Included in Version` [`real_name`]; `indicates`: `node_assigned_to_hierarchy_version`; `supports`: `node_in_multiple_hierarchy_versions` |
| `custom_records_translations:translations` | Translations | Custom Records: Translations | `requires_feature`: `Multi-Language` [`real_name`] |
| `customization_custom_lists:custom_list_values` | Custom List Values | Customization: Custom Lists | `selective_update_key`: `ID` [`real_name`] |
| `customization_custom_lists:custom_list_translations` | Custom List Translations | Customization: Custom Lists | `requires_feature`: `Multi-Language` [`real_name`]; `selective_update_key`: `Language` [`real_name`] |
| `employees_employees:payroll_accrued_times` | Payroll Accrued Times | Employees: Employees | `selective_update_key`: `Payroll Item` [`real_name`]; `requires_feature`: `Payroll` [`real_name`] |
| `employees_employees:address` | Address | Employees: Employees | `selective_update_key`: `Internal ID`, `Label` [`real_name`] |
| `employees_employees:payroll_contributions` | Payroll Contributions | Employees: Employees | `selective_update_key`: `Payroll Item` [`real_name`]; `requires_feature`: `Payroll` [`real_name`] |
| `employees_employees:payroll_deductions` | Payroll Deductions | Employees: Employees | `selective_update_key`: `Payroll Item` [`real_name`]; `requires_feature`: `Payroll` [`real_name`] |
| `employees_employees:direct_deposit` | Direct Deposit | Employees: Employees | `selective_update_key`: `Internal ID` [`real_name`]; `requires_feature`: `Direct Deposit` [`real_name`] |
| `employees_employees:payroll_earnings` | Payroll Earnings | Employees: Employees | `selective_update_key`: `Payroll Item` [`real_name`]; `requires_feature`: `Payroll` [`real_name`] |
| `employees_employees:education` | Education | Employees: Employees |  |
| `employees_employees:emergency_contacts` | Emergency Contacts | Employees: Employees |  |
| `employees_employees:roles` | Roles | Employees: Employees |  |
| `employees_employees:subscriptions` | Subscriptions | Employees: Employees | `selective_update_key`: `Subscriptions` [`real_name`] |
| `employees_expense_reports:expenses` | Expenses | Employees: Expense Reports | `requires_feature`: `Expense Reports` [`real_name`]; `selective_update_key`: `Line Id`, `Ref No.` [`real_name`] |
| `employees_expense_reports:accounting_book_detail` | Accounting Book Detail | Employees: Expense Reports | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `items:bin_numbers` | Bin Numbers | Items | `requires_any_feature`: `Bin Management`, `Advanced Bin/Numbered Inventory Management` [`real_name`]; `selective_update_key`: `Bin Number` [`real_name`]; `available_in_import`: `Assembly/BOM Item`, `Inventory Item` [`real_name`]; `import_order`: `import_bins_before_items_with_bin_numbers` |
| `items:locations` | Locations | Items | `requires_feature`: `Multi-Location Inventory` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Location` [`real_name`]; `available_in_import`: `Assembly/BOM Item`, `Inventory Item` [`real_name`] |
| `items:members` | Members | Items | `available_in_import`: `Assembly/BOM Item`, `Item Group`, `Kit/Package Item` [`real_name`] |
| `items:merchandise_hierarchy` | Merchandise Hierarchy | Items | `available_in_import`: `Inventory Item`, `Lot Numbered Inventory Item`, `Serialized Inventory Item` [`real_name`]; `requires_feature`: `Merchandise Hierarchy` [`real_name`] |
| `items:pricing` | Pricing | Items | `requires_any_feature`: `Multiple Prices`, `Quantity Pricing`, `Multiple Currencies` [`real_name`]; `selective_update_key`: `Currency`, `Quantity`, `Level` (when `depends_on_enabled_features`) [`real_name`] |
| `items:related_items` | Related Items | Items | `requires_feature`: `Web Site` [`real_name`]; `not_available_in_import`: `Non-Inventory Item for Purchase`, `Service Item for Purchase`, `Other Charge Item` [`real_name`] |
| `items:site_category` | Site Category | Items | `requires_feature`: `Web Site` [`real_name`]; `not_available_in_import`: `Non-Inventory Item for Purchase`, `Service Item for Purchase`, `Other Charge Item` [`real_name`] |
| `items:task_templates` | Task Templates | Items | `requires_feature`: `Project Management` [`real_name`]; `available_in_import`: `Service Item for Sale` [`real_name`] |
| `items:translations` | Translations | Items | `requires_feature`: `Multi-Language` [`real_name`]; `selective_update_key`: `Language` [`real_name`] |
| `items:vendors` | Vendors | Items | `requires_feature`: `Multiple Vendors` [`real_name`]; `selective_update_key`: `Vendor` [`real_name`]; `available_in_import`: `Inventory Item`, `Non-Inventory Item for Resale or Purchase`, `Service Item for Resale or Purchase`, `Other Charge Item for Resale or Purchase` [`real_name`]; `available_in_import`: `Assembly/BOM Item` (when `allow_purchase_of_assembly_items_enabled`) [`real_name`] |
| `items_subscription_plan:subscription_plan_members` | Subscription Plan Members | Items: Subscription Plan | `requires_feature`: `SuiteBilling` [`real_name`] |
| `relationships_contacts:address` | Address | Relationships: Contacts | `selective_update_key`: `Internal ID`, `Label` [`real_name`]; `available_in_import`: `Contacts Only` [`real_name`]; `not_available_in_import`: `Customers and Contacts Together`, `Leads and Contacts Together`, `Prospects and Contacts Together` [`real_name`] |
| `relationships_contacts:subscriptions` | Subscriptions | Relationships: Contacts | `selective_update_key`: `Subscription` [`real_name`] |
| `relationships_customers:address` | Address | Relationships: Customers | `selective_update_key`: `Internal ID`, `Label` [`real_name`] |
| `relationships_customers:contact_access` | Contact Access | Relationships: Customers | `available_only_in`: `updates_of_existing_records`; `purpose`: `add_or_update_access_of_existing_contacts`; `selective_update_key`: `Contact` [`real_name`] |
| `relationships_customers:credit_cards` | Credit Cards | Relationships: Customers | `supports`: `adding_new_records`; `not_supported`: `updating_existing_lines` |
| `relationships_customers:currencies` | Currencies | Relationships: Customers | `requires_feature`: `Multiple Currencies` [`real_name`] |
| `relationships_customers:downloads` | Downloads | Relationships: Customers | `requires_feature`: `Sell Files` [`real_name`] |
| `relationships_customers:group_pricing` | Group Pricing | Relationships: Customers | `requires_feature`: `Multiple Prices` [`real_name`] |
| `relationships_customers:item_pricing` | Item Pricing | Relationships: Customers | `requires_feature`: `Multiple Prices` [`real_name`] |
| `relationships_customers:partners` | Partners | Relationships: Customers | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`] |
| `relationships_customers:sales_team` | Sales Team | Relationships: Customers | `requires_feature`: `Team Selling` [`real_name`] |
| `relationships_customers:subscriptions` | Subscriptions | Relationships: Customers | `selective_update_key`: `Subscription` [`real_name`] |
| `relationships_customers:tax_registrations` | Tax Registrations | Relationships: Customers | `requires_feature`: `SuiteTax` [`real_name`] |
| `relationships_groups:group_members` | Group Members | Relationships: Groups | `selective_update_key`: `Internal ID`, `External ID`, `Names` [`real_name`] |
| `relationships_jobs:address` | Address | Relationships: Jobs | `requires_feature_disabled`: `Project Management` [`real_name`]; `selective_update_key`: `Internal ID`, `Label` [`real_name`] |
| `relationships_jobs:resource` | Resource | Relationships: Jobs | `requires_feature`: `Project Management` [`real_name`] |
| `relationships_leads:address` | Address | Relationships: Leads | `selective_update_key`: `Internal ID`, `Label` [`real_name`] |
| `relationships_leads:credit_cards` | Credit Cards | Relationships: Leads | `supports`: `adding_new_records`; `not_supported`: `updating_existing_lines` |
| `relationships_leads:downloads` | Downloads | Relationships: Leads | `requires_feature`: `Sell Files` [`real_name`] |
| `relationships_leads:group_pricing` | Group Pricing | Relationships: Leads | `requires_feature`: `Multiple Prices` [`real_name`] |
| `relationships_leads:item_pricing` | Item Pricing | Relationships: Leads | `requires_feature`: `Multiple Prices` [`real_name`] |
| `relationships_leads:partners` | Partners | Relationships: Leads | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`] |
| `relationships_leads:sales_team` | Sales Team | Relationships: Leads | `requires_feature`: `Team Selling` [`real_name`] |
| `relationships_partners:address` | Address | Relationships: Partners | `selective_update_key`: `Internal ID`, `Label` [`real_name`] |
| `relationships_partners:contact_access` | Contact Access | Relationships: Partners | `available_only_in`: `updates_of_existing_records`; `purpose`: `add_or_update_access_of_existing_contacts`; `selective_update_key`: `Contact` [`real_name`] |
| `relationships_partners:promotion_codes` | Promotion Codes | Relationships: Partners |  |
| `relationships_partners:subscriptions` | Subscriptions | Relationships: Partners | `selective_update_key`: `Subscription` [`real_name`] |
| `relationships_partners:tax_registrations` | Tax Registrations | Relationships: Partners | `requires_feature`: `SuiteTax` [`real_name`] |
| `relationships_prospects:address` | Address | Relationships: Prospects | `selective_update_key`: `Internal ID`, `Label` [`real_name`] |
| `relationships_prospects:credit_cards` | Credit Cards | Relationships: Prospects | `supports`: `adding_new_records`; `not_supported`: `updating_existing_lines` |
| `relationships_prospects:downloads` | Downloads | Relationships: Prospects | `requires_feature`: `Sell Files` [`real_name`] |
| `relationships_prospects:group_pricing` | Group Pricing | Relationships: Prospects | `requires_feature`: `Multiple Prices` [`real_name`] |
| `relationships_prospects:item_pricing` | Item Pricing | Relationships: Prospects | `requires_feature`: `Multiple Prices` [`real_name`] |
| `relationships_prospects:partners` | Partners | Relationships: Prospects | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`] |
| `relationships_prospects:sales_team` | Sales Team | Relationships: Prospects | `requires_feature`: `Team Selling` [`real_name`] |
| `relationships_vendors:address` | Address | Relationships: Vendors | `selective_update_key`: `Internal ID`, `Label` [`real_name`] |
| `relationships_vendors:roles` | Roles | Relationships: Vendors |  |
| `relationships_vendors:subscriptions` | Subscriptions | Relationships: Vendors | `selective_update_key`: `Subscription` [`real_name`] |
| `relationships_vendors:tax_registrations` | Tax Registrations | Relationships: Vendors | `requires_feature`: `SuiteTax` [`real_name`] |
| `rules_setup_foreign_currency_variance_posting_rule:criteria` | Criteria | Rules Setup: Foreign Currency Variance Posting Rule | `requires_feature`: `Foreign Currency Variance Mapping` [`real_name`] |
| `supply_chain_bill_of_materials_revision:bill_of_materials_revision_components` | Bill of Materials Revision Components | Supply Chain: Bill of Materials Revision | `requires_feature`: `Advanced Bill of Materials` [`real_name`] |
| `supply_chain_manufacturing_cost_template:cost_detail` | Cost Detail | Supply Chain: Manufacturing Cost Template | `min_lines_for_new_records`: `1` |
| `supply_chain_manufacturing_routing:routing_steps` | Routing Steps | Supply Chain: Manufacturing Routing | `selective_update_key`: `Operation Sequence` [`real_name`]; `min_lines_for_new_records`: `1` |
| `supply_chain_manufacturing_routing:component_per_operation` | Component per Operation | Supply Chain: Manufacturing Routing |  |
| `support_solutions:solutions` | Solutions | Support: Solutions |  |
| `support_solutions:topics` | Topics | Support: Solutions |  |
| `support_support_cases:escalate_to` | Escalate to | Support: Support Cases |  |
| `transactions_advanced_intercompany_journal_entry:advanced_intercompany_journal_entry_line` | Advanced Intercompany Journal Entry Line | Transactions: Advanced Intercompany Journal Entry | `min_lines`: `4`; `rule`: `debit_and_credit_for_originating_and_receiving_subsidiary`; `rule`: `debits_and_credits_must_balance` |
| `transactions_cash_sale:auth_code` | Auth. Code | Transactions: Cash Sale | `requires_feature`: `Gift Certificates` [`real_name`] |
| `transactions_cash_sale:billable_time` | Billable Time | Transactions: Cash Sale | `selective_update_key`: `related transaction ID` [`real_name`]; `doc_inconsistency`: `placeholder_marker_left_in_notes` [`needs_human_review`] |
| `transactions_cash_sale:items` | Items | Transactions: Cash Sale | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_cash_sale:partners` | Partners | Transactions: Cash Sale | `requires_feature`: `Team Selling` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`]; `doc_inconsistency`: `partners_team_selling_vs_multi_partner_management_elsewhere` [`needs_human_review`] |
| `transactions_cash_sale:sales_team` | Sales Team | Transactions: Cash Sale | `requires_feature`: `Team Selling` [`real_name`] |
| `transactions_cash_sale:shipment` | Shipment | Transactions: Cash Sale | `requires_feature`: `Multiple Shipping Routes` [`real_name`] |
| `transactions_check:accounting_books` | Accounting Books | Transactions: Check | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book` |
| `transactions_check:expenses` | Expenses | Transactions: Check | `selective_update_key`: `Line` [`real_name`] |
| `transactions_check:items` | Items | Transactions: Check | `selective_update_key`: `Item` [`real_name`] |
| `transactions_credit_card_charge:accounting_books` | Accounting Books | Transactions: Credit Card Charge | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Line`, `Order Line` [`real_name`] |
| `transactions_credit_card_charge:expenses` | Expenses | Transactions: Credit Card Charge | `selective_update_key`: `Line` [`real_name`] |
| `transactions_credit_card_charge:items` | Items | Transactions: Credit Card Charge | `selective_update_key`: `Item` [`real_name`] |
| `transactions_credit_card_refund:accounting_books` | Accounting Books | Transactions: Credit Card Refund | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Line`, `Order Line` [`real_name`] |
| `transactions_credit_card_refund:expenses` | Expenses | Transactions: Credit Card Refund | `selective_update_key`: `Line` [`real_name`] |
| `transactions_credit_card_refund:items` | Items | Transactions: Credit Card Refund | `selective_update_key`: `Item` [`real_name`] |
| `transactions_credit_memo:apply` | Apply | Transactions: Credit Memo |  |
| `transactions_credit_memo:items` | Items | Transactions: Credit Memo |  |
| `transactions_custom_transaction:accounting_books` | Accounting Books | Transactions: Custom Transaction |  |
| `transactions_custom_transaction:lines` | Lines | Transactions: Custom Transaction | `requires_feature`: `Custom Transactions` [`real_name`]; `requires_condition`: `at_least_one_custom_transaction_type_exists`; `selective_update_key`: `Line`, `Account` [`real_name`]; `selective_update_key`: `Account` (when `every_line_has_unique_account`) [`real_name`] |
| `transactions_custom_transaction:accounting_book_detail` | Accounting Book Detail | Transactions: Custom Transaction | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_customer_payment:invoices` | Invoices | Transactions: Customer Payment | `selective_update_key`: `Invoice` [`real_name`]; `selective_update_optional_key`: `Line` [`real_name`] |
| `transactions_customer_payment:accounting_book_detail` | Accounting Book Detail | Transactions: Customer Payment | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_estimate:items` | Items | Transactions: Estimate | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_estimate:partners` | Partners | Transactions: Estimate | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`] |
| `transactions_estimate:sales_team` | Sales Team | Transactions: Estimate | `requires_feature`: `Team Selling` [`real_name`] |
| `transactions_estimate:shipment` | Shipment | Transactions: Estimate | `requires_feature`: `Multiple Shipping Routes` [`real_name`] |
| `transactions_inbound_shipment:items` | Items | Transactions: Inbound Shipment |  |
| `transactions_intercompany_journal_entry:intercompany_journal_entry_line` | Intercompany Journal Entry Line | Transactions: Intercompany Journal Entry | `min_lines`: `4`; `rule`: `debit_and_credit_for_originating_and_receiving_subsidiary`; `rule`: `debits_and_credits_must_balance` |
| `transactions_intercompany_journal_entry:accounting_book_detail` | Accounting Book Detail | Transactions: Intercompany Journal Entry | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_inventory_cost_revaluation:cost_components` | Cost Components | Transactions: Inventory Cost Revaluation | `requires_feature`: `Standard Costing` [`real_name`]; `selective_update_key`: `Component`, `Cost Category` [`real_name`] |
| `transactions_inventory_cost_revaluation:accounting_book_detail` | Accounting Book Detail | Transactions: Inventory Cost Revaluation | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_inventory_transfer:adjustments` | Adjustments | Transactions: Inventory Transfer |  |
| `transactions_inventory_transfer:adjustments_inventory_detail` | Adjustments - Inventory Detail | Transactions: Inventory Transfer | `requires_feature`: `Advanced Bin/Numbered inventory Management` [`real_name`] |
| `transactions_invoice:accounting_book_detail` | Accounting Book Detail | Transactions: Invoice | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_invoice:auth_code` | Auth. Code | Transactions: Invoice | `requires_feature`: `Gift Certificates` [`real_name`] |
| `transactions_invoice:billable_time` | Billable Time | Transactions: Invoice | `selective_update_key`: `related transaction ID` [`real_name`]; `doc_inconsistency`: `placeholder_marker_left_in_notes` [`needs_human_review`] |
| `transactions_invoice:items` | Items | Transactions: Invoice | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_invoice:partners` | Partners | Transactions: Invoice | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`] |
| `transactions_invoice:sales_team` | Sales Team | Transactions: Invoice | `requires_feature`: `Team Selling` [`real_name`] |
| `transactions_invoice:shipment` | Shipment | Transactions: Invoice | `requires_feature`: `Multiple Shipping Routes` [`real_name`] |
| `transactions_item_demand_plan:demand_planning` | Demand Planning | Transactions: Item Demand Plan | `fields_vary_by`: `demand_plan_type`; `allowed_values`: `monthly`, `weekly`, `daily` |
| `transactions_item_supply_plan:orders` | Orders | Transactions: Item Supply Plan |  |
| `transactions_journal_entry:journal_entry_line` | Journal Entry Line | Transactions: Journal Entry | `min_lines`: `2`; `rule`: `debits_and_credits_must_balance` |
| `transactions_journal_entry:accounting_book_detail` | Accounting Book Detail | Transactions: Journal Entry | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_opportunity:items` | Items | Transactions: Opportunity | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`] |
| `transactions_opportunity:opportunity` | Opportunity | Transactions: Opportunity |  |
| `transactions_opportunity:partners` | Partners | Transactions: Opportunity | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`] |
| `transactions_opportunity:sales_team` | Sales Team | Transactions: Opportunity | `requires_feature`: `Team Selling` [`real_name`] |
| `transactions_paycheck:contributions` | Contributions | Transactions: Paycheck |  |
| `transactions_paycheck:deductions` | Deductions | Transactions: Paycheck |  |
| `transactions_paycheck:earnings` | Earnings | Transactions: Paycheck |  |
| `transactions_paycheck:expenses` | Expenses | Transactions: Paycheck |  |
| `transactions_paycheck:taxes` | Taxes | Transactions: Paycheck |  |
| `transactions_paycheck:time` | Time | Transactions: Paycheck |  |
| `transactions_period_end_journal_entry:period_end_journal_entry_line` | Period End Journal Entry Line | Transactions: Period End Journal Entry |  |
| `transactions_purchase_order:items` | Items | Transactions: Purchase Order | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_purchase_order:expenses` | Expenses | Transactions: Purchase Order | `requires_preference`: `Allow Expenses on Purchase Orders` [`real_name`]; `requires_separate_csv_file`: `true` |
| `transactions_return_authorization:items` | Items | Transactions: Return Authorization | `selective_update_key`: `Order Line`, `Item` [`real_name`] |
| `transactions_return_authorization:partners` | Partners | Transactions: Return Authorization | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Partner` [`real_name`] |
| `transactions_return_authorization:sales_team` | Sales Team | Transactions: Return Authorization | `requires_feature`: `Team Selling` [`real_name`]; `selective_update_key`: `Employee` [`real_name`] |
| `transactions_sales_order:accounting_book_detail` | Accounting Book Detail | Transactions: Sales Order | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_sales_order:auth_code` | Auth. Code | Transactions: Sales Order | `requires_feature`: `Gift Certificates` [`real_name`] |
| `transactions_sales_order:items` | Items | Transactions: Sales Order | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_sales_order:partners` | Partners | Transactions: Sales Order | `requires_feature`: `Multi-Partner Management` [`real_name`]; `selective_update_key`: `Internal ID`, `External ID`, `Partner` [`real_name`] |
| `transactions_sales_order:sales_team` | Sales Team | Transactions: Sales Order | `requires_feature`: `Team Selling` [`real_name`] |
| `transactions_sales_order:shipment` | Shipment | Transactions: Sales Order | `requires_feature`: `Multiple Shipping Routes` [`real_name`] |
| `transactions_statistical_journal_entry:line` | Line | Transactions: Statistical Journal Entry | `min_lines_for_new_records`: `1` |
| `transactions_subscription:price_book_lines` | Price Book Lines | Transactions: Subscription | `requires_feature`: `SuiteBilling` [`real_name`] |
| `transactions_subscription_change_order:subscription_change_order_items` | Subscription Change Order Items | Transactions: Subscription Change Order | `requires_feature`: `SuiteBilling` [`real_name`]; `selective_update_key`: `Line Number` [`real_name`] |
| `transactions_transfer_order:accounting_books` | Accounting Books | Transactions: Transfer Order |  |
| `transactions_transfer_order:items` | Items | Transactions: Transfer Order | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`] |
| `transactions_vendor_bill:accounting_book_detail` | Accounting Book Detail | Transactions: Vendor Bill | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_vendor_bill:items` | Items | Transactions: Vendor Bill | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_vendor_bill:expenses` | Expenses | Transactions: Vendor Bill | `requires_separate_csv_file`: `true` |
| `transactions_vendor_credit:items` | Items | Transactions: Vendor Credit | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_vendor_credit:expenses` | Expenses | Transactions: Vendor Credit | `requires_separate_csv_file`: `true`; `selective_update_key`: `Line/Order Line` [`real_name`] |
| `transactions_vendor_credit:apply` | Apply | Transactions: Vendor Credit | `requires_separate_csv_file`: `true`; `selective_update_key`: `Apply`, `Line` [`real_name`] |
| `transactions_vendor_credit:accounting_book_detail` | Accounting Book Detail | Transactions: Vendor Credit | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_vendor_payment:bills` | Bills | Transactions: Vendor Payment | `selective_update_key`: `Bill` [`real_name`]; `selective_update_optional_key`: `Line` [`real_name`] |
| `transactions_vendor_payment:accounting_book_detail` | Accounting Book Detail | Transactions: Vendor Payment | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `transactions_vendor_return_authorization:items` | Items | Transactions: Vendor Return Authorization | `selective_update_key`: `Line/Order Line`, `Item` [`real_name`]; `min_lines_for_new_records`: `1` |
| `transactions_vendor_return_authorization:expenses` | Expenses | Transactions: Vendor Return Authorization | `requires_separate_csv_file`: `true`; `selective_update_key`: `Line/Order Line` [`real_name`] |
| `transactions_vendor_return_authorization:accounting_book_detail` | Accounting Book Detail | Transactions: Vendor Return Authorization | `requires_feature`: `Multi-Book Accounting` [`real_name`]; `requires_condition`: `more_than_one_active_accounting_book`; `selective_update_key`: `Accounting Book` [`real_name`] |
| `website_site_category:presentation_items` | Presentation Items | Website: Site Category |  |

## Suspected source errors

> [!SOURCE-ERROR] `placeholder_marker_left_in_notes`
<!-- source-error: transactions_cash_sale:billable_time | - | placeholder_marker_left_in_notes -->

> [!SOURCE-ERROR] `partners_team_selling_vs_multi_partner_management_elsewhere`
<!-- source-error: transactions_cash_sale:partners | - | partners_team_selling_vs_multi_partner_management_elsewhere -->

> [!SOURCE-ERROR] `placeholder_marker_left_in_notes`
<!-- source-error: transactions_invoice:billable_time | - | placeholder_marker_left_in_notes -->
