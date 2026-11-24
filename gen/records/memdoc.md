# Memorized Transaction Definition (`memdoc`)
<!-- id: record:memdoc -->

| Fact | Value |
|---|---|
| Internal ID | `memdoc` |
| Category | transaction |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_functions`: `search`
- `not_supported`: `create`, `copy`
- `created_in`: `ui_only`
- `rule`: `never_posts_in_closed_period_moves_to_next_open`
- `requires_permission`: `override_period_restrictions` (when `allow_posting_in_locked_period`)
- `purpose`: `schedule_for_recurring_memorized_transactions`
- `script_functions`: `read`, `update`, `delete`, `search`
- `doc_inconsistency`: `says_search_only_and_also_read_update_delete`
- `effect`: `edits_and_deletes_tracked_in_system_notes`
- `effect`: `allow_locked_period_posting_works_for_users_without_override`
- `applies_to`: `checks`, `credit_cards`, `purchase_orders`, `bills`, `sales_orders`, `invoices`, `estimates`, `cash_sales`, `journal_entries`, `statement_charges`, `work_orders`, `purchase_requisitions`, `transfer_orders`, `opportunities`, `custom_transactions`
- `not_supported`: `server_scripts_on_automatically_created_transactions`
- `risk`: `create_scripts_skip_automatic_memorized_transactions`
- `not_scriptable`: `system_notes_subtab`, `transaction_subtab`
- `not_supported`: `transform`
- `sublists`: `customdates`
- `value_examples`: `set_action_reminder_and_next_date`

## Body fields

| Field ID | Type | Label | Required | submitField | Read-only | Tags | Facts |
|---|---|---|---|---|---|---|---|
| `action` | select | Action | yes |  |  |  | `purpose`: `memorized_transaction_action`; `options`: `Template Only`, `Reminder`, `Automatic`; `option_meanings`: `template_only:no_transactions_created`, `reminder:remind_before_saving`, `automatic:posts_automatically` |
| `allowpostinginclosedperiod` | checkbox | Allow Posting in Locked Period | no |  |  |  | `purpose`: `allow_posting_in_locked_period`; `requires_permission`: `override_period_restrictions`; `effect`: `posts_in_locked_period_regardless_of_initiator`; `cannot`: `post_in_closed_period`; `effect`: `closed_period_posts_to_next_open_period` |
| `indefinite` | checkbox | Indefinite | no |  |  |  | `purpose`: `number_of_transactions_or_indefinite`; `mutually_exclusive_members`: `numberremaining`, `indefinite`; `help_text_shared_with`: `memdoc.numberremaining` |
| `isinactive` | checkbox | Inactive | no |  |  |  | `purpose`: `inactivate_memorized_transaction`; `true_means`: `no_more_transactions_created` |
| `name` | text | Name | yes |  |  |  | `purpose`: `memorized_transaction_name`; `default_behavior`: `entity_name_from_transaction`; `caveat`: `does_not_rename_original_transaction` |
| `nextdate` | date | Next Date | no |  |  |  | `purpose`: `next_date`; `required_when`: `automatic_action` |
| `nexttrandate` | date | Next Transaction Date | no |  |  |  | `purpose`: `next_transaction_posting_date`; `applies_when`: `overridetransactiondate` |
| `numberremaining` | integer | Number Remaining | no |  |  |  | `purpose`: `number_of_transactions_or_indefinite`; `mutually_exclusive_members`: `numberremaining`, `indefinite`; `help_text_shared_with`: `memdoc.indefinite` |
| `overridetransactiondate` | checkbox | Override Transaction Date | no |  |  |  | `purpose`: `override_transaction_date`; `requires_together`: `nextdate`, `nexttrandate`; `value_examples`: `generated_7_16_posted_7_20` |
| `repeatevery` | integer | Repeat Every | no |  |  |  | `purpose`: `repeat_every` |
| `timeperiod` | text | Time Period | no |  |  |  | `purpose`: `time_period` |
| `transactiontype` | select | Transaction Type | no |  | yes |  | `purpose`: `memorized_transaction_type`; `value_source`: `originating_transaction_form` |
| `updateaddresses` | checkbox | Update Addresses | no |  |  |  | `purpose`: `update_addresses`; `true_means`: `current_contact_details_at_creation`; `false_means`: `contact_details_as_first_memorized` |
| `updateprices` | checkbox | Update Prices | no |  |  |  | `purpose`: `update_prices`; `true_means`: `current_prices_at_creation`; `false_means`: `prices_as_first_memorized` |
| `externalid` | text | External ID | no |  |  |  |  |
| `id` | integer | Internal ID | no |  |  |  |  |
| `inprogress` | checkbox | Being Processed | no |  |  |  |  |
| `internalid` | integer | Internal ID | no |  |  |  |  |
| `origbinactive` | checkbox |  | no |  |  |  |  |
| `transactiontemplate` | select | Memorized Transaction Template | no |  |  |  |  |

## Sublist `customdates` (Custom Dates)

| Field ID | Type | Label | Required | Tags | Facts |
|---|---|---|---|---|---|
| `customdateid` | integer | Custom Date Id | no |  |  |
| `memdoc` | select | Memorized Transaction Definition | no |  |  |
| `nextdate` | date | Next Date | no |  |  |
| `nexttrandate` | date | Next Transaction Date | no |  |  |

## Search filters

| Filter ID | Type | Label | Tags |
|---|---|---|---|
| `action` | select | Action |  |
| `allowpostinginclosedperiod` | checkbox | Allow posting in locked period |  |
| `amount` | currency | Amount |  |
| `externalid` | select | External ID |  |
| `externalidstring` | text | External ID (Text) |  |
| `hasremaining` | checkbox | Has Remaining |  |
| `indefinite` | checkbox | Indefinite |  |
| `internalid` | select | Internal ID |  |
| `internalidnumber` | integer | Internal ID (Number) |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `nextdate` | date | Next Date |  |
| `nexttrandate` | date | Next Transaction Date |  |
| `numberremaining` | integer | Number Remaining |  |
| `overridetransactiondate` | checkbox | Override Transaction Date |  |
| `repeatevery` | integer | Repeat Every |  |
| `timeperiod` | select | Time Period |  |
| `transactiontype` | select | Transaction Type |  |
| `updateaddresses` | checkbox | Update Addresses |  |
| `updateprices` | checkbox | Update Prices |  |

## Search columns

| Column ID | Type | Label | Tags |
|---|---|---|---|
| `action` | text | Action |  |
| `allowpostinginclosedperiod` | checkbox | Allow Posting In Locked Period |  |
| `amount` | currency | Amount |  |
| `externalid` | select | External ID |  |
| `indefinite` | checkbox | Indefinite |  |
| `internalid` | select | Internal ID |  |
| `isinactive` | checkbox | Inactive |  |
| `name` | text | Name |  |
| `nextdate` | date | Next Date |  |
| `nexttrandate` | date | Transaction Date |  |
| `numberremaining` | text | Number Remaining |  |
| `overridetransactiondate` | checkbox | Override Transaction Date |  |
| `repeatevery` | integer | Repeat Every |  |
| `timeperiod` | text | Time Period |  |
| `transactiontype` | text | Transaction Type |  |
| `updateaddresses` | checkbox | Update Addresses |  |
| `updateprices` | checkbox | Update Prices |  |

## Search joins

| Join ID | Label | Joined record |
|---|---|---|
| `transaction` | Transaction | Transaction |
| `user` | User | Employee |

## Suspected source errors

> [!SOURCE-ERROR] `says_search_only_and_also_read_update_delete`
<!-- source-error: record:memdoc | - | says_search_only_and_also_read_update_delete -->
