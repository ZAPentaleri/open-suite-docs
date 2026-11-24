# SuiteFlow client triggers
<!-- id: topic:suiteflow.client_triggers -->

Part of: [SuiteFlow workflows](suiteflow.md)

Related: [Server triggers](suiteflow.server_triggers.md), [Trigger execution order](suiteflow.trigger_execution.md),
[Form and field actions](suiteflow.field_actions.md), [ClientScript](../script-types/ClientScript.md).

Client triggers run in the user's browser while a form is open, so they react to what the user is
doing rather than to loads and saves on the server. There are five:

| Trigger | When it fires | Closest client script entry point |
|---|---|---|
| Before User Edit | Once per form load, after the form has loaded and before any edit | `pageInit` |
| Before Field Edit | When the user leaves a field they edited; used for validation | `validateField` |
| After Field Edit | After the user changes a field's value | `fieldChanged` |
| After Field Sourcing | After dependent fields have been sourced | none named in the spec |
| Before User Submit | After the user clicks Save, before Before Record Submit runs on the server | none named in the spec |

The entry points are documented on the [ClientScript](../script-types/ClientScript.md) page.

For the field-level triggers (Before Field Edit, After Field Edit, After Field Sourcing) you must pick
the client field the action watches.

Validating in the browser is friendlier than validating on the server: the user keeps what they
entered, whereas a server-side error page discards it.

## Before User Edit

Runs once each time the form loads. Prefer the server-side Before Record Load trigger where it can do
the same job, because reloading the page re-runs the Before User Edit actions.

Which trigger can do what:

| Action | Before Record Load | Before User Edit |
|---|---|---|
| Set a field value on a new record | Yes | Yes |
| Set a field value on an existing record | No | Yes |
| Set the display type | Yes | Yes |
| Make a field mandatory | Yes | Yes |
| Add or remove buttons | Yes | No |
| Lock the record | Yes | No |

> [!SOURCE-ERROR] Sources disagree on whether Before User Edit can set the inline display type and the
> display label: the Before Record Load page and the quick reference say yes, while the Before User Edit
> page says no. Test it in your account before relying on either answer.
<!-- source-error: topic:suiteflow.client_triggers | - | inline_display_type_and_display_label_on_before_user_edit -->

## Before Field Edit

Fires when the user leaves a field they changed, which makes it the place for field validation. A Return
User Error action here shows a popup again and again until the field holds an acceptable value.

> [!SOURCE-ERROR] The source describes this trigger both as running before the user finishes editing the
> field and as running after the user leaves it. The spec does not settle which; observe the behaviour
> in your account.
<!-- source-error: topic:suiteflow.client_triggers | - | described_as_before_completing_and_as_after_leaving_field -->

## After Field Edit

Fires after the user changes the value of a field. It also fires when a Before Record Load action
changes the field the action is watching.

> [!SOURCE-ERROR] In running text the source calls this trigger "After User Edit", which is not a
> trigger name. It means After Field Edit.
<!-- source-error: topic:suiteflow.client_triggers | - | trigger_called_after_user_edit_in_text -->

## After Field Sourcing

Fires once the fields that depend on the watched field have been sourced, whether from the same record
or from a joined one. Use it to set values based on what was just sourced.

> [!SOURCE-ERROR] One description of this trigger speaks of "required" fields where dependent (sourced)
> fields are meant. The trigger waits for dependent fields to be sourced.
<!-- source-error: topic:suiteflow.client_triggers | - | sourcing_trigger_says_required_not_dependent_fields -->

> [!SOURCE-ERROR] The source page for this trigger refers to it as After Field Edit in one place, which
> looks like a typo. It means After Field Sourcing.
<!-- source-error: topic:suiteflow.client_triggers | - | page_calls_trigger_after_field_edit -->

## Before User Submit

Runs in the browser after the user clicks Save, before the server's Before Record Submit trigger. It
is the right trigger for checks that should stop the save without throwing away the user's input.
