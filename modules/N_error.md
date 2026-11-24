# N/error
<!-- id: N/error -->

`N/error` builds custom SuiteScript errors. `error.create` only constructs the error object; it does
not throw it. Your script decides when to `throw` it, which is what aborts the current execution.
Every created error gets an ID automatically, and when it is thrown its `stack` lists the method
calls that led there.

- **Availability:** server scripts only.
- **Since:** the source does not state a version for the module itself; its members date from
  2015.2.
- **Load it as:** `define(['N/error'], (error) => { ... })`
- A [`SuiteScriptError`](#suitescripterror-object) carries `cause`, `id`, `message`, `name`,
  `notifyOff`, `stack` and `type`.
- Related: [N/log](N_log.md) (errors you catch are usually written to the execution log).

## Members

| Member | Kind | Summary |
|---|---|---|
| [`error.create(options)`](#errorcreateoptions) | function | Build (not throw) a custom error |
| [`SuiteScriptError`](#suitescripterror-object) | object | The error object |
| [`error.Type`](#errortype) | enum | Standard error names |

## Functions

### error.create(options)
<!-- id: N/error.create -->

Returns a new [`SuiteScriptError`](#suitescripterror-object). Nothing is thrown at this point; the
script must `throw` the returned object itself for it to stop execution.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.message` | string | Yes | `null` |
| `options.name` | string | Yes | |
| `options.notifyOff` | boolean | No | `false` |

- `options.message`: the human-readable text. It appears in the Details column of the execution
  log. Since 2015.2.

> [!SOURCE-ERROR] The source marks `message` as required yet also gives it a default of `null`.
> These cannot both hold; always pass a message string, since leaving it out is listed as an error.
<!-- source-error: N/error.create | options.message | marked_required_but_default_null -->

- `options.name`: the error code. Use your own code (for example a project-specific identifier
  such as `my_custom_error`) or one of the [`error.Type`](#errortype) values. Since 2015.2.
- `options.notifyOff`: leave it `false` to have the error emailed to the users listed on the
  script's Unhandled Errors subtab; set `true` to suppress that email. Since 2015.2.

**Returns:** `error.SuiteScriptError`, the new error, not yet thrown.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.message` or `options.name` was not supplied.
- `WRONG_PARAMETER_TYPE`: `options.message` or `options.name` is not a string, or
  `options.notifyOff` is not a boolean.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2015.2

## Objects

### SuiteScriptError (object)
<!-- id: N/error.SuiteScriptError -->

A custom error meant to abort the script when thrown. Returned by
[`error.create`](#errorcreateoptions). All its properties are read-only. Server scripts; since
2015.2.

#### SuiteScriptError.cause
<!-- id: N/error.SuiteScriptError#cause -->

`string`, read-only. The cause of the error. Since 2015.2.

#### SuiteScriptError.notifyOff
<!-- id: N/error.SuiteScriptError#notifyOff -->

Typed `string` in the spec, read-only. `true` means the email notification for this error is
suppressed. It reflects `options.notifyOff` passed to `error.create`. Since 2015.2.

> [!SOURCE-ERROR] The source's type row lists `string` for this property, while it behaves as a
> true/false flag that mirrors the boolean `notifyOff` option. Compare it as a boolean, and verify
> if your logic depends on its exact type.
<!-- source-error: N/error.SuiteScriptError#notifyOff | - | type_row_says_string_for_boolean -->

#### SuiteScriptError.type
<!-- id: N/error.SuiteScriptError#type -->

`string`, read-only. Always `error.SuiteScriptError`, which lets you tell these errors apart from
other exceptions. Since 2015.2.

#### SuiteScriptError.id
<!-- id: N/error.SuiteScriptError#id -->

`string`, read-only. The error's ID, generated automatically when the error is created. Since
2015.2.

#### SuiteScriptError.name
<!-- id: N/error.SuiteScriptError#name -->

`string`, read-only. The error code, taken from `options.name` of `error.create`. Since 2015.2.

#### SuiteScriptError.message
<!-- id: N/error.SuiteScriptError#message -->

`string`, read-only. The error text, taken from `options.message`; it is what shows in the Details
column of the execution log. Since 2015.2.

#### SuiteScriptError.stack
<!-- id: N/error.SuiteScriptError#stack -->

`string[]`, read-only. The method calls leading to the point where the error was thrown, most
recent first. Since 2015.2.

## Enums

### error.Type
<!-- id: N/error.Type -->

Names of standard SuiteScript errors. You can use them as `options.name` in `error.create` or
compare them with the `name` of a caught error. A given type only applies when the account has the
settings that the error concerns. Since 2015.2.

> [!SOURCE-ERROR] The source's list has four problems. (1) Its longest value is broken by a stray
> space and is therefore missing from the values below; the spec cannot say exactly what it is.
> (2) It lists `INVALID_TEPORAL_UNIT` [sic], apparently a misspelling of "temporal"; the misspelt
> form is kept because it may be what the runtime uses. (3) It lists `INVALID_ALPHA_VALUE`, but the
> color-creating API throws a doubled form of that name. (4) It lists `NO_MEASURES_DEFINED`, but
> the selector API throws a singular form. When matching on these names, test against the code
> your script actually receives.
<!-- source-error: N/error.Type | - | longest_value_broken_by_space_and_absent_from_values -->
<!-- source-error: N/error.Type | - | lists_invalid_teporal_unit_spelling -->
<!-- source-error: N/error.Type | - | lists_invalid_alpha_value_but_createcolor_throws_doubled -->
<!-- source-error: N/error.Type | - | lists_no_measures_defined_but_selector_throws_singular -->

| Value |
|---|
| `AT_LEAST_ONE_EXPRESSION_IS_NEEDED` |
| `BUTTONS_MUST_INCLUDE_BOTH_A_LABEL_AND_VALUE` |
| `CAN_REFERENCE_ONLY_PERSISTED_DATASET` |
| `CAN_SELECT_ONLY_ONE_DEFAULT_CHOICE` |
| `CANNOT_CREATE_RECORD_DRAFT_OF_EXISTING_RECORD` |
| `CANNOT_CREATE_RECORD_INSTANCE` |
| `CANNOT_DETERMINE_TYPE_FOR_ALIAS` |
| `CANNOT_DETERMINE_VALUE_FOR_ALIAS` |
| `CANNOT_RESUBMIT_SUBMITTED_ASYNC_PIVOT_TASK` |
| `CANNOT_RESUBMIT_SUBMITTED_ASYNC_QUERY_TASK` |
| `CANNOT_RESUBMIT_SUBMITTED_ASYNC_SEARCH_TASK` |
| `CANNOT_RESUBMIT_SUBMITTED_ASYNC_SUITEQL_TASK` |
| `CREDIT_CARD_NUMBER_MUST_CONTAIN_BETWEEN_13_AND_20_DIGITS` |
| `CREDIT_CARD_NUMBER_MUST_CONTAIN_ONLY_DIGITS` |
| `DATASET_NAME_IS_MISSING` |
| `DEFAULT_CHOICE_IS_MISSING` |
| `EACH_VIEW_MUST_HAVE_AN_ID` |
| `EACH_VIEW_MUST_HAVE_A_NAME` |
| `EMPTY_KEY_NOT_ALLOWED` |
| `EMPTY_KEY_NOT_ALLOWED_FOR_1` |
| `EXPRESSION_CANNOT_BE_SPECIFIED_WHEN_USING_COUNT_DISTINCT_AGGREGATION` |
| `EXPRESSION_MUST_BE_SPECIFIED_WHEN_USING_OTHER_THAN_COUNT_DISTINCT_AGGREGATION` |
| `EXPRESSIONS_CANNOT_BE_SPECIFIED_WHEN_USING_OTHER_THAN_COUNT_DISTINCT_AGGREGATION` |
| `EXPRESSIONS_MUST_BE_SPECIFIED_WHEN_USING_COUNT_DISTINCT_AGGREGATION` |
| `FAILED_AN_UNEXPECTED_ERROR_OCCURRED` |
| `FIELD_1_ALREADY_CONTAINS_A_SUBRECORD_YOU_CANNOT_CALL_CREATESUBRECORD` |
| `FIELD_1_CANNOT_BE_EMPTY` |
| `FIELD_1_IS_NOT_A_SUBRECORD_FIELD` |
| `FIELD_MUST_CONTAIN_A_VALUE` |
| `FORM_VALIDATION_FAILED_YOU_CANNOT_SUBMIT_THIS_RECORD` |
| `FORM_VALIDATION_FAILED_YOU_CANNOT_CREATE_THIS_SUBRECORD` |
| `HISTORY_IS_ONLY_AVAILABLE_FOR_THE_LAST_30_DAYS` |
| `ID_CANNOT_HAVE_MORE_THAN_N_CHARACTERS` |
| `IDENTIFIERS_CAN_CONTAIN_ONLY_DIGITS_ALPHABETIC_CHARACTERS_OR__WITH_NO_SPACES` |
| `INVALID_AGGREGATE_TYPE` |
| `INVALID_AGGREGATION` |
| `INVALID_ALGORITHM` |
| `INVALID_ALPHA_VALUE` (see the source-error note) |
| `INVALID_ASPECT_TYPE` |
| `INVALID_CERTIFICATE_TYPE` |
| `INVALID_CHART_TYPE` |
| `INVALID_COLOR_VALUE` |
| `INVALID_COLUMN_ALIAS` |
| `INVALID_COLUMN_FOR_SORTING` |
| `INVALID_CONFIGURATION_UNABLE_TO_CHANGE_REQUIRE_CONFIGURATION_FOR_1` |
| `INVALID_CONFIGURATION_UNABLE_TO_CHANGE_REQUIRE_CONFIGURATION_WITHOUT_A_CONTEXT` |
| `INVALID_CONFLICT_RESOLUTION_1` |
| `INVALID_CURRENCY` |
| `INVALID_CUSTOM_VIEW_VALUE` |
| `INVALID_DATASET_ID` |
| `INVALID_DATE_ID` |
| `INVALID_DATE_VALUE_MUST_BE_ON_OR_AFTER_1CUTOFF_DATE` |
| `INVALID_DATE_VALUE_MUST_BE_1` |
| `INVALID_DATE_OBJECT` |
| `INVALID_DIRECTION_FOR_SORTING` |
| `INVALID_EMAILS_FOUND` |
| `INVALID_EXPRESSION` |
| `INVALID_FIELD_CONTEXT` |
| `INVALID_FIELD_ID` |
| `INVALID_FIELD_INDEX` |
| `INVALID_FIELD_VALUE` |
| `INVALID_FILTER_FIELD_FOR_CURRENT_VIEW` |
| `INVALID_FLD_VALUE` |
| `INVALID_FONT_SIZE` |
| `INVALID_FONT_STYLE` |
| `INVALID_FONT_WEIGHT` |
| `INVALID_FORMULA_TYPE` |
| `INVALID_GETSELECTOPTION_FILTER_OPERATOR` |
| `INVALID_HTTP_METHOD` |
| `INVALID_ID_PREFIX` |
| `INVALID_IMAGE` |
| `INVALID_KEY_OR_REF` |
| `INVALID_KEY_TYPE` |
| `INVALID_LOCALE` |
| `INVALID_NUMBER_MUST_BE_BETWEEN_1_AND_2` |
| `INVALID_NUMBER_MUST_BE_GREATER_THAN_1` |
| `INVALID_NUMBER_MUST_BE_LOWER_THAN_1` |
| `INVALID_NUMBER_OR_PERCENTAGE` |
| `INVALID_OPERATION` |
| `INVALID_OPERATOR` |
| `INVALID_OR_UNSUPPORTED_RECORD_TYPE_1` |
| `INVALID_OWNER_CATEGORY` |
| `INVALID_PAGE_INDEX` |
| `INVALID_PAGE_RANGE` |
| `INVALID_PERIOD_ADJUSTMENT` |
| `INVALID_PERIOD_CODE` |
| `INVALID_PERIOD_TYPE` |
| `INVALID_POSITION` |
| `INVALID_RCRD_TYPE` |
| `INVALID_RETURN_TYPE_EXPECTED_1` |
| `INVALID_SCRIPT_OPERATION_ON_READONLY_SUBLIST_FIELD` |
| `INVALID_SEARCH_OPERATOR` |
| `INVALID_SEARCH_TYPE` |
| `INVALID_SIGNATURE` |
| `INVALID_SIGNATURE_TAG` |
| `INVALID_SORT` |
| `INVALID_SORT_LOCALE` |
| `INVALID_STACKING_TYPE` |
| `INVALID_SUBLST_OPERATION` |
| `INVALID_SUBRECORD_MERGE` |
| `INVALID_SUITEAPP_APPLICATION_ID` |
| `INVALID_TASK_TYPE` |
| `INVALID_TEPORAL_UNIT` [sic] |
| `INVALID_TEXT_ALIGN` |
| `INVALID_TEXT_DECORATION_LINE` |
| `INVALID_TEXT_DECORATION_STYLE` |
| `INVALID_TOTAL_LINE` |
| `INVALID_TYPE_1_USE_2` |
| `INVALID_UI_OBJECT_TYPE` |
| `INVALID_UNIT` |
| `INVALID_URL_SPACES_ARE_NOT_ALLOWED_IN_THE_URL` |
| `INVALID_URL_URL_MUST_START_WITH_HTTP_HTTPS_FTP_OR_FILE` |
| `INVALID_WORKBOOK_ID` |
| `METHOD_IS_ONLY_ALLOWED_FOR_MATRIX_FIELD` |
| `MISSING_MANDATORY_FIELDS` |
| `MISSING_REQD_ARGUMENT` |
| `MUTUALLY_EXCLUSIVE_ARGUMENTS` |
| `NAME_CANNOT_BE_EMPTY` |
| `NAME_CANNOT_HAVE_MORE_THAN_N_CHARACTERS` |
| `NEITHER_ARGUMENT_DEFINED` |
| `NO_ASPECTS_DEFINED` |
| `NO_CHILDREN_DEFINED` |
| `NO_COLUMN_DEFINED` |
| `NO_DATASET_DEFINED` |
| `NO_DIMENSION_ITEM_DEFINED` |
| `NO_ELEMENTS_DEFINED` |
| `NO_MEASURES_DEFINED` (see the source-error note) |
| `NO_RULE_DEFINED` |
| `NO_SELECTORS_DEFINED` |
| `NO_SORT_BY_DEFINED` |
| `NON_KATAKANA_DATA_FOUND` |
| `NOT_SUPPORTED_ON_CURRENT_SUBRECORD` |
| `NOTICE_THE_CREDIT_CARD_APPEARS_TO_BE_INCORRECT` |
| `OPERATOR_ARITY_MISMATCH` |
| `OPERATION_IS_NOT_ALLOWED` |
| `PASSWORD_CANNOT_HAVE_MORE_THAN_N_CHARACTERS` |
| `PHONE_NUMBER_SHOULD_HAVE_SEVEN_DIGITS_OR_MORE` |
| `PIVOT_DOES_NOT_EXIST` |
| `PLEASE_ENTER_A_VALID_FROM_START_DATE_IN_MMYYYY_FORMAT` |
| `PLEASE_ENTER_AN_EXPIRATION_DATE_IN_MMYYYY_FORMAT` |
| `PLEASE_INCLUDE_THE_AREA_CODE_FOR_PHONE_NUMBER` |
| `PROPERTY_VALUE_CONFLICT` |
| `READ_ONLY_PROPERTY` |
| `RELATIONSHIP_ALREADY_USED` |
| `SCRIPT_EXECUTION_USAGE_LIMIT_EXCEEDED` |
| `SELECT_OPTION_ALREADY_PRESENT` |
| `SELECT_OPTION_NOT_FOUND` |
| `SERVER_SIDE_VALIDATION_FAILED` |
| `SIGNATURE_VERIFICATION_FAILED` |
| `SSS_ARGUMENT_DISCREPANCY` |
| `SSS_DUPLICATE_ALIAS` |
| `SSS_INVALID_ACTION_ID` |
| `SSS_INVALID_API_USAGE` |
| `SSS_INVALID_COUNTRY_ID` |
| `SSS_INVALID_CURRENCY_ID` |
| `SSS_INVALID_FORMAT_TYPE` |
| `SSS_INVALID_GETSELECTOPTION_FILTER_OPERATOR` |
| `SSS_INVALID_MACRO_ID` |
| `SSS_INVALID_READ_SIZE` |
| `SSS_INVALID_SEARCH_RESULT_INDEX` |
| `SSS_INVALID_SEGMENT_SEPARATOR` |
| `SSS_INVALID_SRCH_OPERATOR` |
| `SSS_INVALID_SUBLIST` |
| `SSS_INVALID_SUBLIST_OPERATION` |
| `SSS_INVALID_TYPE_ARG` |
| `SSS_INVALID_UI_OBJECT_TYPE` |
| `SSS_INVALID_URL` |
| `SSS_METHOD_IS_ONLY_ALLOWED_FOR_MATRIX_FIELD` |
| `SSS_METHOD_IS_ONLY_ALLOWED_FOR_MULTISELECT_FIELD` |
| `SSS_METHOD_IS_ONLY_ALLOWED_FOR_SELECT_FIELD` |
| `SSS_MISSING_ALIAS` |
| `SSS_MISSING_REQD_ARGUMENT` |
| `SSS_NOT_YET_SUPPORTED` |
| `SSS_RECORD_DOES_NOT_SATISFY_CONDITION` |
| `SSS_RECORD_TYPE_MISMATCH` |
| `SSS_SEARCH_FOR_EACH_LIMIT_EXCEEDED` |
| `SSS_SEARCH_RESULT_LIMIT_EXCEEDED` |
| `SSS_SUBLIST_DOESNT_SUPPORT_MOVING_LINES` |
| `SSS_TAG_CANNOT_BE_EMPTY` |
| `SSS_TAX_REGISTRATION_REQUIRED` |
| `SSS_UNSUPPORTED_METHOD` |
| `TABLE_DOES_NOT_EXIST` |
| `THAT_RECORD_IS_NOT_EDITABLE` |
| `THE_FIELD_1_CONTAINED_MORE_THAN_THE_MAXIMUM_NUMBER__2__OF_CHARACTERS_ALLOWED` |
| `THE_OPTIONS_ARE_MUTUALLY_EXCLUSIVE_1_2_ARG2_` |
| `TOO_MANY_RESULTS` |
| `TRANSLATION_HANDLE_IS_IN_AN_ILLEGAL_STATE` |
| `UNHANDLED_ERRORS_ON_RESTORE` |
| `UNKNOWN_CONTEXT_TYPE` |
| `UNKNOWN_PARAM` |
| `UNSUPPORTED_COLOR` |
| `VALUE_1_OUTSIDE_OF_VALID_MINMAX_RANGE_FOR_FIELD2` |
| `WORKBOOK_NAME_IS_MISSING` |
| `WRONG_PARAMETER_TYPE` |
| `WS_INVALID_REFERENCE_KEY_1` |
| `WS_NO_PERMISSIONS_TO_SET_VALUE` |
| `YOU_HAVE_ATTEMPTED_AN_UNSUPPORTED_ACTION` |

## Examples

A user event script that blocks saving a vendor bill whose memo is empty when the amount is large.
The error carries a project-specific code, and the unhandled-error email is suppressed because the
user sees the message on screen.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/error'], (error) => {
  const LIMIT = 25000;

  const beforeSubmit = (context) => {
    const bill = context.newRecord;
    const total = Number(bill.getValue({ fieldId: 'usertotal' })) || 0;
    const memo = bill.getValue({ fieldId: 'memo' });
    if (total > LIMIT && !memo) {
      throw error.create({
        name: 'AP_MEMO_REQUIRED_OVER_LIMIT',
        message: `Bills above ${LIMIT} need a memo explaining the spend.`,
        notifyOff: true
      });
    }
  };

  return { beforeSubmit };
});
```

Telling a SuiteScript error apart from a plain JavaScript exception when cleaning up after a failed
step, and logging the useful parts:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/error', 'N/log'], (error, log) => {
  const runStep = (step) => {
    try {
      return step();
    } catch (e) {
      if (e.type === 'error.SuiteScriptError') {
        log.error({ title: `${e.name} (${e.id})`, details: e.message });
      } else {
        log.error({ title: 'Unexpected failure', details: String(e) });
      }
      throw error.create({ name: 'BATCH_STEP_FAILED', message: 'A batch step failed; see the log.' });
    }
  };

  return { runStep };
});
```
