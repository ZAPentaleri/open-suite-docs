# Entry Point Script Validation
<!-- id: topic:entry_point_validation -->

Part of: [Script Deployment](script_deployment.md)

See also: [SuiteScript 2.1 JSDoc validation](jsdoc_validation.md), error list:
[catalog:entry_point_validation_errors](../gen/catalogs/entry_point_validation_errors.md).

NetSuite checks a SuiteScript 2.x entry point script before it lets you use it. A file that fails the
check cannot get a script record and cannot be attached to a form; sometimes it cannot even be
uploaded to the File Cabinet.

## When the check runs

The file is validated:

- when it is uploaded to the File Cabinet,
- when a script record is created for it, and
- when a client script is attached to a custom form.

Errors can therefore appear at upload, at script record creation, at form attachment, and also when
you edit a file that has already been uploaded or attached. Data gathered from the file at upload is
saved and reused when the script record is created.

An edit to a file that is already attached is rejected if it would introduce a validation error.

## Vocabulary

| Term | Meaning |
|---|---|
| JSDoc tags | The `@N...` annotations in the file's header comment |
| JSDoc tag values | What follows each tag, such as `2.1` or `UserEventScript` |
| Script type interface | The object the `define` callback returns |
| Entry points | The keys of that object, such as `beforeSubmit` |
| Entry point functions | The functions those keys point to |
| Entry point function definitions | Where those functions are written in the file |

## What a valid file looks like

Required JSDoc tags: `@NApiVersion` and `@NScriptType`.

- The only valid `@NApiVersion` value listed is `2.1`. Update SuiteScript 1.0, 2.0 and `2.x` scripts to
  2.1 by release 2028.2. (The text of the invalid-version message mentions `2.X`, `2.x`, `2.0` and
  `2.1`; `2.x` and `2.X` mean the same thing.)
- Valid `@NScriptType` values:

  | | | |
  |---|---|---|
  | [BundleInstallationScript](../script-types/BundleInstallationScript.md) | ConsolidatedRateAdjustorPlugin | CustomGLPlugin |
  | [CustomTool](../script-types/CustomTool.md) | [ClientScript](../script-types/ClientScript.md) | EmailCapturePlugin |
  | [MapReduceScript](../script-types/MapReduceScript.md) | [MassUpdateScript](../script-types/MassUpdateScript.md) | PaymentGatewayPlugin |
  | PluginTypeImpl | [Portlet](../script-types/Portlet.md) | PromotionsPlugin |
  | [Restlet](../script-types/Restlet.md) | [ScheduledScript](../script-types/ScheduledScript.md) | ShippingPartnersPlugin |
  | [Suitelet](../script-types/Suitelet.md) | TaxCalculationPlugin | [UserEventScript](../script-types/UserEventScript.md) |
  | [WorkflowActionScript](../script-types/WorkflowActionScript.md) | | |

Structure the file must have:

1. Exactly one `define` call.
2. No direct references to API objects or enums at the top level of the `define` callback; wrap such
   references in a function.
3. The callback returns one script type interface.
4. The interface has at least one entry point.
5. Every entry point has a function.
6. The entry point functions are in the same file.
7. The interface matches the `@NScriptType` value.
8. No JavaScript syntax errors.

When an entry point and its function share a name, you can use property shorthand in the returned
object (`return { beforeSubmit }`).

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/record', 'N/log'], (record, log) => {
  // Enum read inside a function, not at the top level of the callback.
  const watchedType = () => record.Type.PURCHASE_ORDER;

  const afterSubmit = (context) => {
    if (context.newRecord.type !== watchedType()) {
      return;
    }
    log.audit({ title: 'PO saved', details: `id ${context.newRecord.id}` });
  };

  return { afterSubmit };
});
```

## Errors

The codes and their triggers are listed in
[catalog:entry_point_validation_errors](../gen/catalogs/entry_point_validation_errors.md). In general,
a validation error is raised when a required element is missing or malformed. The cases the spec spells
out:

| Code | Raised when | When you see it |
|---|---|---|
| `FAILED_TO_VALIDATE_SCRIPT_FILE` | `@NApiVersion` is missing, or global variables are not initialized properly | At upload |
| `INVALID_API_VERSION` | `@NApiVersion` has a value that is not accepted | At upload |
| `CANNOT_CHANGE_API_VERSION` | You change `@NApiVersion` in a file already attached to a script record or form | On edit |
| `MULTIPLE_DEFINE_CALLS` | The file calls `define` more than once | |
| `WRONG_SCRIPT_TYPE` | `@NScriptType` does not match (is incompatible with) the entry points the file returns | At upload |
| `MISSING_SCRIPT_TYPE` | A script record is requested for a file that has no `@NScriptType` | At script record creation; the upload itself succeeds |
| `SS_V2_FILE_USED_FOR_FORM_SCRIPTS_MUST_IMPLEMENT_CLIENT_SCRIPT_TYPE` | A file that is not a client script is attached to a form | At form attachment |
| `SCRIPT_OF_API_VERSION_20_MUST_IMPLEMENT_A_SCRIPT_TYPE_INTERFACE` | The `define` callback has no return statement, or the file uses `require` instead of `define` | At upload |

The source abbreviates the last code in places; the full spelling is in the catalog. A related code,
`SCRIPT_OF_API_VERSION_20_CANNOT_IMPLEMENT_MORE_THAN_ONE_SCRIPT_TYPE_INTERFACES`, covers a file
whose entry points belong to more than one script type.

> [!SOURCE-ERROR] In one detail section the source writes the full code of the "cannot implement more
> than one script type interface" error with "SCRIPT" misspelt as "SCRIT". Use the spelling given in
> the error catalog and check the actual message your account returns.
<!-- source-error: topic:entry_point_validation | - | full_code_spelled_scrit_type_in_detail_section -->

You cannot change a file's script type once it is attached. To switch types, create a new file and a
new script record.

### A missing @NScriptType tag

Leaving out `@NScriptType` does not stop the upload, so a successful upload does not prove the file is
valid. The error shows up later, when you request a script record or attach the file to a form.

> [!SOURCE-ERROR] The source quotes the `MISSING_SCRIPT_TYPE` message text with "2.0" in its summary
> table and "2.1" in the detailed description. Treat the version in that message as unreliable.
<!-- source-error: topic:entry_point_validation | - | missing_script_type_text_says_2_0_in_table_2_1_in_detail -->

The source also disagrees about which code this case produces: one page says `MISSING_SCRIPT_TYPE`
(at script record creation), another says `FAILED_TO_VALIDATE_SCRIPT_FILE` (at script record creation
or form attachment).

> [!SOURCE-ERROR] Two source pages name different error codes for a file without `@NScriptType`:
> `MISSING_SCRIPT_TYPE` on one and `FAILED_TO_VALIDATE_SCRIPT_FILE` on the other. The spec does not say
> which is right, so handle both codes.
<!-- source-error: topic:entry_point_validation | - | nscripttype_missing_error_differs_between_pages -->
