# Custom module tutorial
<!-- id: topic:custom_modules.tutorial -->

Part of: [SuiteScript 2.1 custom modules](custom_modules.md)

See also: [UserEventScript](../script-types/UserEventScript.md), [N/record](../modules/N_record.md).

The tutorial extends an existing user event script so that, whenever a new employee record is
created, a phone call record is logged for that employee. The phone call logic moves out of the user
event script into a custom module, which the user event script then loads. This page summarises what
the walkthrough establishes; the parent page has a worked example of a module and a script that loads
it.

## Before you start

You need the user event script from the earlier user event tutorial, already deployed. The tutorial
builds on that script rather than starting from scratch.

## The module

- The module returns one entry point, named `schedule`, that creates the phone call record. Whatever
  function a module returns must be defined in the same file.
- An entry point can read a fixed value held in the module, not only its arguments.
- Using JSDoc tags in custom modules is recommended, although `@NApiVersion` and `@NModuleScope` are
  both optional there. Older scripts may carry the legacy version values `2.0`, `2.x` or `2.X`.
- A custom module needs no script record and no deployment record. As soon as the file is uploaded and
  a script references it, it is available.
- Upload it from Documents > Files > SuiteScripts with **Add File**.

## Wiring it into the user event script

- Add the module to the `define` dependency list of the user event script, naming it by its file name.
- The tutorial then shows the complete revised user event script.
- Upload the revised script file the same way, through Documents > Files > SuiteScripts. Uploading a
  file with the same name as an existing one asks you to confirm the overwrite.

## The disabled field

The user event script also disables a field on the form. A disabled field is shown greyed out and
cannot be edited.

> [!SOURCE-ERROR] The tutorial text credits the `beforeSubmit` entry point with disabling the field,
> but in the script it is `beforeLoad` that does it. Treat `beforeLoad` as the entry point that makes
> this change.
<!-- source-error: topic:custom_modules.tutorial | - | says_beforesubmit_disables_field_beforeload_does -->

## Checking the result

Create an employee and confirm that a phone call record was created for them. If you do not see it,
check the filter on the phone call list: it may be hiding calls that belong to other employees.
