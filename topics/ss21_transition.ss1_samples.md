# SuiteScript 1.0 conversion samples
<!-- id: topic:ss21_transition.ss1_samples -->

Part of: [Transitioning to SuiteScript 2.1](ss21_transition.md)

See also: [SuiteScript 1.0 conversion samples (searches and imports)](ss21_transition.ss1_samples_more.md),
[Converting SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md).

The vendor's transition material walks through a set of 1.0 scripts next to their 2.1 rewrites.
This page records, for each pair, which API mappings it demonstrates and where the sample itself
goes wrong, so that anyone converting legacy code by analogy knows which parts to trust. The vendor
samples are not reproduced here; an original before-and-after pair closes the page.

## Custom module: item receipt lines

Mappings demonstrated:

| 1.0 | 2.1 |
|---|---|
| `getLineItemValue` | [`Record.getSublistValue`](../modules/N_record.md) |
| `setLineItemValue` | [`Record.setSublistValue`](../modules/N_record.md) |
| `nlapiSendEmail` | [`email.send`](../modules/N_email.md) |

In the 2.1 version the first sublist line is still addressed as line 1, even though 2.x counts
lines from 0.

> [!SOURCE-ERROR] Carrying the 1.0 line number 1 into 2.x points at the second line, not the first.
> When you convert line-based code, shift every index down by one so the first line is 0.
<!-- source-error: topic:ss21_transition.ss1_samples | - | item_receipt_sample_keeps_line_1_though_2x_zero_based -->

The accompanying notes also state that JSDoc tags were added to the module snippet.

> [!SOURCE-ERROR] That remark about added JSDoc tags does not match the module snippet as the source
> presents it. Do not assume a converted custom module already carries the tags it needs; check them.
<!-- source-error: topic:ss21_transition.ss1_samples | - | says_jsdoc_added_for_module_snippet -->

## User event: set the sales rep

Mapping demonstrated: `nlapiGetNewRecord` becomes `context.newRecord` on the entry point context
(see [UserEventScript](../script-types/UserEventScript.md)). The header comment of the 2.1 sample
opens with a single asterisk.

> [!SOURCE-ERROR] A comment opened with `/*` is not a JSDoc block, so the annotations inside it are
> not in a JSDoc comment. Open the block holding `@NApiVersion` and `@NScriptType` with `/**`.
<!-- source-error: topic:ss21_transition.ss1_samples | - | sales_rep_sample_uses_single_star_jsdoc_block -->

## User event: end-of-month promotion

Mappings demonstrated: `getExecutionContext` becomes
[`runtime.executionContext`](../modules/N_runtime.md), and the `form` argument of the 1.0 function
becomes `context.form`. The two versions do not behave the same: the 1.0 script checks that it is
running in the user interface, and the 2.1 rewrite leaves that check out. If you convert similar
code, keep the context check.

The 2.1 sample's dependency list names modules that its code never calls.

> [!SOURCE-ERROR] Some modules in the sample's `define` list are unused. The list is not a guide to
> what the logic requires; load only the modules your converted code actually calls.
<!-- source-error: topic:ss21_transition.ss1_samples | - | listed_imports_not_used_by_sample -->

The sample's debug logging call is also misspelled.

> [!SOURCE-ERROR] As printed, the logging call names a function that does not exist and would fail
> when reached. The correct call is `log.debug`.
<!-- source-error: topic:ss21_transition.ss1_samples | - | log_debug_misspelled -->

## Suitelet: simple form

Mappings demonstrated: `nlapiCreateForm` becomes
[`serverWidget.createForm`](../modules/N_ui_serverWidget.md), and the single 1.0 `setLayoutType`
call is split across two enums, `FieldLayoutType` and `FieldBreakType`. Along the way the sample
illustrates three things:

- fields added by a Suitelet use IDs that start with `custpage`;
- a sublist of the inline editor kind;
- a loop that reads submitted sublist lines with the request's
  [`getSublistValue`](../modules/N_https.md).

The rewrite omits some fields and options that the 1.0 form had, so the two are not equivalent line
for line.

## Scheduled: fulfill and bill orders

Mappings demonstrated: `nlobjSearchFilter` becomes [`search.createFilter`](../modules/N_search.md),
`nlobjSearchColumn` becomes `search.createColumn`, and `nlapiTransformRecord` becomes
[`record.transform`](../modules/N_record.md). The 2.1 version decides between creating an invoice
and a cash sale the other way round from the 1.0 version.

> [!SOURCE-ERROR] The branches are swapped in the rewrite: the condition that led to one
> transaction type in 1.0 leads to the other in 2.1. Keep the original condition when converting, and
> confirm which transaction each branch is meant to produce.
<!-- source-error: topic:ss21_transition.ss1_samples | - | fulfill_bill_sample_inverts_invoice_cash_sale_choice -->

The description of this sample also gives its entry point as `onRequest`.

> [!SOURCE-ERROR] `onRequest` is not a scheduled script entry point; a scheduled script runs through
> `execute`. See [ScheduledScript](../script-types/ScheduledScript.md).
<!-- source-error: topic:ss21_transition.ss1_samples | - | says_onrequest_entry_point_for_scheduled -->

## RESTlet: create, read, delete

Mappings demonstrated: `nlapiLoadRecord` becomes [`record.load`](../modules/N_record.md),
`nlapiCreateRecord` becomes `record.create`, and `nlapiDeleteRecord` becomes `record.delete`. Here,
too, the description calls the entry point `onRequest`.

> [!SOURCE-ERROR] A RESTlet exposes `get`, `post`, `put` and `delete` entry points; `onRequest` is not
> one of them. See [Restlet](../script-types/Restlet.md).
<!-- source-error: topic:ss21_transition.ss1_samples | - | says_onrequest_entry_point_for_restlet -->

## Portlet: simple form

Mappings demonstrated: `setLayoutType` becomes two field methods, `updateLayoutType` and
`updateBreakType`, and positional arguments become options objects (see
[Portlet](../script-types/Portlet.md) and [N/ui/serverWidget](../modules/N_ui_serverWidget.md)).

## Mass update: set the department

Mappings demonstrated: `nlapiLoadRecord` becomes `record.load`, `getSetting` becomes
[`Script.getParameter`](../modules/N_runtime.md), and `nlapiSubmitRecord` becomes
[`Record.save`](../modules/N_record.md). The script parameter ID in the sample is misspelled; the
intended ID is `custscript_dept_update`.

> [!SOURCE-ERROR] Because of the typo, the sample's parameter lookup would not find the parameter
> defined on the script record. The intended ID is `custscript_dept_update`; in your own code, copy
> parameter IDs exactly as they appear on the script record.
<!-- source-error: topic:ss21_transition.ss1_samples | - | mass_update_sample_misspells_custscript_parameter -->

## Client: show the user's profile

Mappings demonstrated: `nlapiGetUser` becomes [`runtime.getCurrentUser`](../modules/N_runtime.md);
`nlapiGetRole`, `nlapiGetDepartment` and `nlapiGetLocation` become the `role`, `department` and
`location` properties of the returned user; and a browser `alert` is replaced by
[`message.create`](../modules/N_ui_message.md) followed by showing the message. The sample refers to
a bare `INFORMATION` constant that it never defines.

> [!SOURCE-ERROR] Nothing in the sample brings `INFORMATION` into scope, so the reference would fail.
> The message type is a value of the module's enum, `message.Type.INFORMATION`; reference it through
> the enum.
<!-- source-error: topic:ss21_transition.ss1_samples | - | profile_sample_uses_undefined_information_constant -->

## Client: disable fields

Mapping demonstrated: `nlapiDisableField` becomes an `isDisabled` property. The sample's notes name
that property `Record.isDisabled`, while its code sets the property on a field object; in the
modules the property exists on fields, for example
[`Field.isDisabled`](../modules/N_currentRecord.md) in N/currentRecord, and no record-level
`isDisabled` exists.

> [!SOURCE-ERROR] Notes and code disagree on which object carries `isDisabled`. Follow the code: get
> the field and set its `isDisabled` property. The source material itself does not settle the point,
> so verify the behaviour in your account if it matters.
<!-- source-error: topic:ss21_transition.ss1_samples | - | notes_name_Record.isDisabled_code_uses_Field.isDisabled -->

The notes also say the record is obtained with `record.load`, but the code works with the current
record.

> [!SOURCE-ERROR] For a client script changing the page the user has open, the code's approach is the
> relevant one: work on the current record (from [N/currentRecord](../modules/N_currentRecord.md) or
> the entry point context), not a separately loaded copy.
<!-- source-error: topic:ss21_transition.ss1_samples | - | disable_fields_notes_say_record_load_code_uses_currentrecord -->

## Client: default field values

Mappings demonstrated: `nlapiGetFieldValue` becomes [`Record.getValue`](../modules/N_record.md),
`nlapiGetFieldText` becomes `Record.getText`, and `nlapiSetFieldText` becomes `Record.setText`. The
notes claim the code uses `setText`, but the code actually calls `setValue`.

> [!SOURCE-ERROR] The notes and the code use different setters. A converted `nlapiSetFieldText`
> corresponds to `setText`; reach for `setValue` only when you are setting the internal value rather
> than the display text.
<!-- source-error: topic:ss21_transition.ss1_samples | - | default_values_notes_say_settext_code_uses_setvalue -->

## Bundle installation: check a feature, create an account

Mappings demonstrated:

| 1.0 | 2.1 |
|---|---|
| `toversion` argument | `params.toVersion` |
| `fromversion` argument | `params.fromVersion` |
| `nlapiCreateRecord` | [`record.create`](../modules/N_record.md) |
| `nlapiSubmitRecord` | `Record.save` |
| `getFeature` | [`runtime.isFeatureInEffect`](../modules/N_runtime.md) |
| `nlobjError` | [`error.create`](../modules/N_error.md) |

The sample shows an installation being stopped with an error when a feature it depends on is turned
off. Its 2.1 version has no `afterInstall` entry point, and it uses N/record without listing that
module among its dependencies.

> [!SOURCE-ERROR] The rewrite loses the original's `afterInstall` entry point and calls N/record
> without loading it. A faithful conversion keeps every entry point and lists every module it uses in
> `define`.
<!-- source-error: topic:ss21_transition.ss1_samples | - | afterinstall_dropped_and_n_record_import_unlisted -->

## An original conversion pair

A 1.0 client `pageInit` function that locks a support case's priority for everyone except one role
(the role ID is made up):

```js
function lockPriorityForOtherRoles(type) {
  var SUPPORT_MANAGER_ROLE = 1013;
  if (type === 'edit' && Number(nlapiGetRole()) !== SUPPORT_MANAGER_ROLE) {
    nlapiDisableField('priority', true);
  }
}
```

In 2.1, `type` becomes `context.mode`, the role is read from the current user, and disabling is a
property of the field object rather than a function call.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/runtime'], (runtime) => {
  const SUPPORT_MANAGER_ROLE = 1013;

  const pageInit = (context) => {
    if (context.mode !== 'edit' || Number(runtime.getCurrentUser().role) === SUPPORT_MANAGER_ROLE) {
      return;
    }
    const priority = context.currentRecord.getField({ fieldId: 'priority' });
    if (priority) {
      priority.isDisabled = true;
    }
  };

  return { pageInit };
});
```
