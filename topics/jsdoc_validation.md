# SuiteScript 2.1 JSDoc validation
<!-- id: topic:jsdoc_validation -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

See also: [Entry Point Script Validation](entry_point_validation.md).

A SuiteScript 2.x file begins with a JSDoc comment block whose `@N...` tags tell NetSuite how to
treat it. NetSuite reads that block with a JSDoc 3 parser. You can also download JSDoc 3 yourself and
generate your own documentation from your comments.

## Writing the block

- The block opens with `/**` and closes with `*/`.
- Each tag is written as `@key value`.
- These forms are **not** valid: a block opened with a single star (`/*`), tags in `//` line
  comments, and a tag value with no line break after it.

## Tags

| Tag | Required? | Purpose |
|---|---|---|
| `@NApiVersion` | Yes, in entry point scripts | The API version of the file |
| `@NScriptType` | Yes, in entry point scripts | The script type the file implements |
| `@NApiVersion` in a custom module | Optional | Fixes the module's version so it does not conflict with the version of the script that loads it |
| `@NModuleScope` | Optional | Which scripts may load the module (see below) |

The required tags are checked when a script record is requested for the file, and the API version is
validated from the JSDoc block on the script record. Separately, an account preference can make 2.x
server scripts run as 2.1.

### @NScriptType values

| | | | |
|---|---|---|---|
| [BundleInstallationScript](../script-types/BundleInstallationScript.md) | ConsolidatedRateAdjustorPlugin | CustomGLPlugin | [ClientScript](../script-types/ClientScript.md) |
| [CustomTool](../script-types/CustomTool.md) | EmailCapturePlugin | [EventSubscriber](../script-types/EventSubscriber.md) | [MapReduceScript](../script-types/MapReduceScript.md) |
| [MassUpdateScript](../script-types/MassUpdateScript.md) | PaymentGatewayPlugin | PluginTypeImpl | [Portlet](../script-types/Portlet.md) |
| PromotionsPlugin | [Restlet](../script-types/Restlet.md) | [ScheduledScript](../script-types/ScheduledScript.md) | ShippingPartnersPlugin |
| [Suitelet](../script-types/Suitelet.md) | TaxCalculationPlugin | [UserEventScript](../script-types/UserEventScript.md) | [WorkflowActionScript](../script-types/WorkflowActionScript.md) |

> [!SOURCE-ERROR] This list includes `EventSubscriber`, but the list on the
> [entry point validation](entry_point_validation.md) page leaves it out. The source does not say
> which list is complete; check the [EventSubscriber](../script-types/EventSubscriber.md) page and
> test in your account before relying on either.
<!-- source-error: topic:jsdoc_validation | - | nscripttype_lists_differ_on_eventsubscriber -->

## @NModuleScope

`@NModuleScope` controls which scripts may load a custom module and whether its source code is
visible. The default is `SameAccount`.

| Value | Who can load it | Source visible? | Loading is refused when | Typical use |
|---|---|---|---|---|
| `SameAccount` | Scripts in the same bundle and the same source account | Yes | A module from a different source account imports it | An ISV bundle with private logic |
| `TargetAccount` | As above, plus the target account | No | The importing script is native to neither the source nor the target account | Public APIs for modules in the target account |
| `Public` | Any script | No | The script is not installed in the account | Open-source code |

Guidance:

- Set the scope explicitly in development accounts (which are limited to 5 users). When several
  developer accounts work together, a module that other accounts depend on must be `Public`, because
  `SameAccount` blocks modules from other accounts.
- Bundled 2.1 scripts that use classes must have `SameAccount` scope.
- Review module scopes before you deploy a SuiteApp.

SuiteBundler is still supported but gets no new features; use Copy to Account or SDF to move
customizations instead.

```js
/**
 * @NApiVersion 2.1
 * @NModuleScope SameAccount
 */
define(['N/log'], (log) => {
  // A custom module shared by scripts in the same bundle and source account.
  const roundToCents = (amount) => Math.round(amount * 100) / 100;

  const describeDiscount = (gross, net) => {
    const saved = roundToCents(gross - net);
    log.debug({ title: 'Discount', details: `${saved}` });
    return saved;
  };

  return { roundToCents, describeDiscount };
});
```
