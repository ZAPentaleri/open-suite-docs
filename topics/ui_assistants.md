# Working with UI Objects
<!-- id: topic:ui_assistants -->

Part of: [Developer guide](developer_guide.md)

See also: [N/ui/serverWidget](../modules/N_ui_serverWidget.md),
[Custom pages: assistants](custom_pages.assistants.md).

This topic is about assistants: multi-step pages, built with
[N/ui/serverWidget](../modules/N_ui_serverWidget.md), that walk a user through a task and look like
NetSuite's own wizards.

## What an assistant looks like

NetSuite's built-in assistants show the pattern. Two you can open to compare:

- the SuiteBundler assistant, at Customization > SuiteBundler > Create Bundle;
- the Import Assistant, at Setup > Import/Export > Import CSV Records.

The visible parts of an assistant are:

- the assistant title,
- the list of steps,
- fields arranged in field groups,
- an optional splash screen.

## Steps

Steps can be set up in two ways:

- **ordered:** the user must complete them in sequence;
- **any order:** the user can jump between steps freely.

If a step reports an error, the user cannot move on to the next step.

> [!SOURCE-ERROR] The source contradicts itself on layout: its text says the steps are shown
> horizontally, while its annotated screenshot says vertically. For the layout tied to each step
> mode, rely on the `Assistant.isNotOrdered` description in
> [N/ui/serverWidget](../modules/N_ui_serverWidget.md) and check the rendered page.
<!-- source-error: topic:ui_assistants | - | steps_said_horizontal_callout_says_vertical -->

## Example

A Suitelet that draws the first screen of a three-step warehouse cycle-count assistant: title,
ordered steps, a splash panel and one field group. Handling of later steps is left out.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/ui/serverWidget'], (serverWidget) => {
  const onRequest = (context) => {
    const wizard = serverWidget.createAssistant({ title: 'Cycle Count Setup' });
    wizard.addStep({ id: 'custpage_step_scope', label: 'Choose bins' });
    wizard.addStep({ id: 'custpage_step_team', label: 'Assign counters' });
    wizard.addStep({ id: 'custpage_step_confirm', label: 'Confirm' });
    wizard.setSplash({
      title: 'Before you start',
      text1: 'Pick the bins to count in this cycle.',
      text2: 'Counts lock the bins until they are posted.'
    });
    wizard.addFieldGroup({ id: 'custpage_grp_scope', label: 'Scope' });
    wizard.addField({
      id: 'custpage_bin_note',
      type: serverWidget.FieldType.TEXTAREA,
      label: 'Notes for counters',
      container: 'custpage_grp_scope'
    });
    context.response.writePage(wizard);
  };

  return { onRequest };
});
```
