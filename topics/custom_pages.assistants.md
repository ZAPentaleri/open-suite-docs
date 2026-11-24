# Custom assistants
<!-- id: topic:custom_pages.assistants -->

Part of: [SuiteScript 2.1 custom pages](custom_pages.md)

See also: [N/ui/serverWidget](../modules/N_ui_serverWidget.md), [Suitelet](../script-types/Suitelet.md).

An assistant is a page that leads a user through a task in steps. You build it in a
[Suitelet](../script-types/Suitelet.md) with N/ui/serverWidget: create it with
[`serverWidget.createAssistant()`](../modules/N_ui_serverWidget.md), which returns a
[`serverWidget.Assistant`](../modules/N_ui_serverWidget.md) object.

## Restrictions

- Assistants are not supported in externally available Suitelets.
- As with every custom page, scripts have no direct access to the DOM.

## Building blocks

| Component | Added with |
|---|---|
| Fields | [`Assistant.addField()`](../modules/N_ui_serverWidget.md) |
| Field groups | [`Assistant.addFieldGroup()`](../modules/N_ui_serverWidget.md) |
| Sublists | [`Assistant.addSublist()`](../modules/N_ui_serverWidget.md) |
| Steps | [`Assistant.addStep()`](../modules/N_ui_serverWidget.md) |

- The navigation buttons are generated for you; you do not add them yourself.
- The only sublist type an assistant offers is the inline editor (`INLINEEDITOR`).
- A sublist can show the results of a saved search.
- Field groups on an assistant can be collapsed (ordinary forms do not support this).
- A step's help text is set through [`AssistantStep.helpText`](../modules/N_ui_serverWidget.md).

## Step layout

[`Assistant.isNotOrdered`](../modules/N_ui_serverWidget.md) decides how the steps are presented:

| Steps | Shown |
|---|---|
| Ordered (sequential) | Vertically, in a panel down the left side |
| Unordered | Horizontally, across the top |

## How to build one

1. Create the assistant and add its steps.
2. Build the page content for each step.
3. In the Suitelet, handle both requests for every step: GET shows the step, POST processes what the
   user submitted on it.

## Moving between steps

On a POST, find out where the user is going with
[`Assistant.getNextStep()`](../modules/N_ui_serverWidget.md). It returns the
[`serverWidget.AssistantStep`](../modules/N_ui_serverWidget.md) that follows from the user's last action
(for example the step after the one just submitted). Call
[`Assistant.getLastStep()`](../modules/N_ui_serverWidget.md) first, as recommended: it returns the step
on which the user submitted that action, so you can read the submitted values from it (for example with
[`AssistantStep.getValue()`](../modules/N_ui_serverWidget.md)) before deciding what comes next.
[`Assistant.getLastAction()`](../modules/N_ui_serverWidget.md) returns the last action itself as a
string; the source does not list its possible values.

Other members that help here: [`Assistant.isFinished()`](../modules/N_ui_serverWidget.md) tells you
whether all steps are complete (the completion message in
[`Assistant.finishedHtml`](../modules/N_ui_serverWidget.md) is then shown),
[`Assistant.currentStep`](../modules/N_ui_serverWidget.md) holds the step being shown, and
[`Assistant.sendRedirect()`](../modules/N_ui_serverWidget.md) manages the assistant's redirects. The
source describes `currentStep` as settable to any step but marks it read-only in its type row; see the
N/ui/serverWidget page before relying on assigning it.

## Example

A two-step assistant for logging a returned pallet. On each POST it reads the submitted step, logs the
step the user is heading to, and redirects so the assistant shows its next page.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget', 'N/log'], (serverWidget, log) => {
  const buildAssistant = () => {
    const assistant = serverWidget.createAssistant({ title: 'Pallet Return Check-in' });
    assistant.addStep({ id: 'custpage_step_pallet', label: 'Identify pallet' });
    assistant.addStep({ id: 'custpage_step_condition', label: 'Record condition' });
    assistant.addField({
      id: 'custpage_pallet_tag',
      label: 'Pallet tag',
      type: serverWidget.FieldType.TEXT
    });
    assistant.finishedHtml = '<p>Pallet checked in.</p>';
    return assistant;
  };

  const onRequest = (context) => {
    const assistant = buildAssistant();
    if (context.request.method === 'GET') {
      context.response.writePage({ pageObject: assistant });
      return;
    }
    if (assistant.isFinished()) {
      context.response.writePage({ pageObject: assistant });
      return;
    }
    const submitted = assistant.getLastStep();
    const next = assistant.getNextStep();
    log.debug({
      title: `Step ${submitted.label} submitted`,
      details: `Next: ${next ? next.label : 'none'}; tag ${submitted.getValue({ id: 'custpage_pallet_tag' })}`
    });
    assistant.sendRedirect({ response: context.response });
  };

  return { onRequest };
});
```
