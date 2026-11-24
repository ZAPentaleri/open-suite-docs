# SuiteFlow trigger execution order
<!-- id: topic:suiteflow.trigger_execution -->

Part of: [SuiteFlow workflows](suiteflow.md)

Related: [Server triggers](suiteflow.server_triggers.md), [Client triggers](suiteflow.client_triggers.md),
[Workflow initiation](suiteflow.initiation.md), [N/workflow](../modules/N_workflow.md).

When a script and a workflow work on the same record, knowing the order in which a workflow evaluates
its actions and transitions explains most surprises. You can always check what actually happened, and
in which order, in the workflow execution log.

## Per user event

| What the user does | Triggers that run |
|---|---|
| Creates or opens a record | Before Record Load |
| Works in the form in the browser | Client triggers |
| Saves | Before Record Submit, then After Record Submit |

Beyond this sequence of record events there is no fixed order among triggers.

From the moment a workflow is initiated, the server triggers follow each other as Before Record Load,
Before Record Submit, After Record Submit. A workflow initiated on Before Record Submit therefore never
runs Before Record Load.

## Inside a state

Actions and transitions both consist of properties, a trigger and conditions. When a record enters a
state, the workflow:

1. runs the entry actions (only the first time the record enters the state);
2. evaluates the entry transitions;
3. runs the actions whose trigger is the one on which the state was entered;
4. evaluates the transitions for that trigger;
5. runs the exit actions just before the record leaves.

For a given trigger, actions always run before transitions.

- An entry action is skipped when its trigger is not valid for the server trigger on which the state
  was entered.
- The next state is entered on the server trigger that is currently running. If that is Before Record
  Submit, the target state's Before Record Load actions are skipped.
- Form changes made on Before Record Load survive a transition that happens during the same load.
- Reloading the record runs the server triggers again, but not the entry actions.

## Transitions

- A transition runs on the exit of its source state and on the entry of its target state.
- Transitions are checked in order; the first one whose condition is true runs, and then the target
  state's actions run.
- If no transition applies, the instance stays dormant until the next trigger arrives.
- Transitions cannot run on client triggers.

A transition can be conditioned on:

- the condition builder;
- a custom formula;
- a saved search;
- a button click (the button must first be added with an Add Button action);
- another workflow or state;
- a schedule.

When a transition has several conditions, all of them must be true.

Formulas are written in SuiteScript for client triggers and in SQL for server triggers.

## Ending a workflow

- An **exit state** is a state with no transitions; a workflow can have several. When the record enters
  one, only the actions of the trigger on which it was entered run, and then the workflow ends. Later
  server triggers and scheduled actions do not run there.
- A state with the **Do Not Exit Workflow** flag keeps the instance alive, so the workflow never
  completes. The flag only applies to states without transitions. It suits single-state workflows and
  records that must stay locked, such as rejected ones.
- If a state without transitions has the Do Not Exit flag, the instance finishes its work there but
  does not end.
- You control how a workflow ends with this flag and with the trigger of the transition that leads into
  the exit state.

## Avoiding duplicate instances

If a workflow initiates on view or update with Before Record Load or All, simply reopening the record
can create extra instances. Instead:

- initiate on Before Record Submit or After Record Submit;
- or initiate the workflow from SuiteScript (for example with
  [`workflow.initiate`](../modules/N_workflow.md)) after the record has been created;
- and keep Before Record Load for preparing the user interface.

## Seeing action IDs

With the Show Internal IDs preference turned on, action IDs appear in the execution log, the context
panel and the action group window.
