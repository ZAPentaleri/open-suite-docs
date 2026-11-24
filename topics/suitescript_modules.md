# SuiteScript 2.1 Modules
<!-- id: topic:suitescript_modules -->

SuiteScript 2.x organizes its API into modules (`N/record`, `N/search`, and so on). A script names
the modules it needs as dependencies in `define` (or `require`) and receives each one as a callback
argument. This page covers the general rules; each module has its own reference page, listed at the
end.

## Loading modules

- Load only the modules the script actually uses.
- Each module's reference describes what it does, which script types support it, and which
  permissions it needs.
- Using a module in a script type that does not support it fails with `UNEXPECTED_ERROR` or
  `INSUFFICIENT_PERMISSION`.

## Notes on particular modules

- **[N/sso](../modules/N_sso.md)** is no longer supported as of release 2025.1.
- **N/commerce modules** (such as [N/commerce/recordView](../modules/N_commerce_recordView.md))
  require SuiteCommerce or SuiteCommerce Advanced 2019.2 or later. See
  [N/commerce Modules](commerce_modules.md).
- **[N/certificateControl](../modules/N_certificateControl.md)** works with the certificates managed
  under Setup > Company > Certificates.
- **[N/manufacturing/productionCharges](../modules/N_manufacturing_productionCharges.md)**: the
  source's module overview gives no supported script types or permissions for it.

> [!SOURCE-ERROR] The source's module overview row for N/manufacturing/productionCharges is
> incomplete: it lists neither supported script types nor required permissions. Check the module's
> own page and test in your target script type.
<!-- source-error: topic:suitescript_modules | - | productioncharges_row_lacks_script_types_and_permissions -->

## Module pages in this reference

| | | |
|---|---|---|
| [N/action](../modules/N_action.md) | [N/auth](../modules/N_auth.md) | [N/cache](../modules/N_cache.md) |
| [N/certificateControl](../modules/N_certificateControl.md) | [N/commerce/recordView](../modules/N_commerce_recordView.md) | [N/compress](../modules/N_compress.md) |
| [N/config](../modules/N_config.md) | [N/crypto](../modules/N_crypto.md) | [N/crypto/certificate](../modules/N_crypto_certificate.md) |
| [N/crypto/random](../modules/N_crypto_random.md) | [N/currency](../modules/N_currency.md) | [N/currentRecord](../modules/N_currentRecord.md) |
| [N/dataset](../modules/N_dataset.md) | [N/datasetLink](../modules/N_datasetLink.md) | [N/documentCapture](../modules/N_documentCapture.md) |
| [N/email](../modules/N_email.md) | [N/encode](../modules/N_encode.md) | [N/error](../modules/N_error.md) |
| [N/file](../modules/N_file.md) | [N/format](../modules/N_format.md) | [N/format/i18n](../modules/N_format_i18n.md) |
| [N/http](../modules/N_http.md) | [N/https](../modules/N_https.md) | [N/https/clientCertificate](../modules/N_https_clientCertificate.md) |
| [N/keyControl](../modules/N_keyControl.md) | [N/llm](../modules/N_llm.md) | [N/log](../modules/N_log.md) |
| [N/machineTranslation](../modules/N_machineTranslation.md) | [N/manufacturing/productionCharges](../modules/N_manufacturing_productionCharges.md) | [N/pgp](../modules/N_pgp.md) |
| [N/piremoval](../modules/N_piremoval.md) | [N/plugin](../modules/N_plugin.md) | [N/portlet](../modules/N_portlet.md) |
| [N/query](../modules/N_query.md) | [N/record](../modules/N_record.md) | [N/recordContext](../modules/N_recordContext.md) |
| [N/redirect](../modules/N_redirect.md) | [N/render](../modules/N_render.md) | [N/runtime](../modules/N_runtime.md) |
| [N/scriptTypes/restlet](../modules/N_scriptTypes_restlet.md) | [N/search](../modules/N_search.md) | [N/sftp](../modules/N_sftp.md) |
| [N/sso](../modules/N_sso.md) | [N/suiteAppInfo](../modules/N_suiteAppInfo.md) | [N/task](../modules/N_task.md) |
| [N/task/accounting/recognition](../modules/N_task_accounting_recognition.md) | [N/transaction](../modules/N_transaction.md) | [N/translation](../modules/N_translation.md) |
| [N/ui/dialog](../modules/N_ui_dialog.md) | [N/ui/message](../modules/N_ui_message.md) | [N/ui/serverWidget](../modules/N_ui_serverWidget.md) |
| [N/url](../modules/N_url.md) | [N/util](../modules/N_util.md) | [N/workbook](../modules/N_workbook.md) |
| [N/workflow](../modules/N_workflow.md) | [N/xml](../modules/N_xml.md) | |

See also [N/ui Modules](ui_modules.md) for the `N/ui` group.

## Example

A client script that depends on exactly the two modules it uses, in the same order as the callback
parameters. It warns when a project's estimated work exceeds a threshold.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ClientScript
 */
define(['N/ui/message', 'N/log'], (message, log) => {
  const LIMIT_HOURS = 400;

  const fieldChanged = (context) => {
    if (context.fieldId !== 'estimatedtimeoverride') {
      return;
    }
    const hours = Number(context.currentRecord.getValue({ fieldId: 'estimatedtimeoverride' })) || 0;
    if (hours > LIMIT_HOURS) {
      message.create({
        type: message.Type.WARNING,
        title: 'Large estimate',
        message: `${hours} hours is above the ${LIMIT_HOURS}-hour review threshold.`,
        duration: 8000
      }).show();
      log.debug({ title: 'Estimate warning', details: hours });
    }
  };

  return { fieldChanged };
});
```
