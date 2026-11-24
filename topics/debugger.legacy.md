# Legacy debugger for 1.0 and 2.0
<!-- id: topic:debugger.legacy -->

Part of: [SuiteScript Debugger](debugger.md)

Sub-topics:
- [Script Debugger Interface](debugger.legacy_interface.md)

The legacy Script Debugger handles SuiteScript 1.0 and 2.0 scripts and core plug-in implementations.
SuiteScript 2.1 scripts use the [2.1 debugger](debugger.ss2_1.md) instead.

You need the **SuiteScript** permission at **Full** level, and you open the tool from
**Customization > Scripting > Script Debugger**. Like every debugging session, it runs on the debugger
domain against your real data: anything the script writes is actually saved.

## Debugging code on demand

On-demand debugging is for 1.0 or 2.0 code that has no deployment, such as a code sample you are
trying out.

- A 2.0 snippet written as a [`define(...)`](../globals.md) module must be rewritten to call
  [`require(...)`](../globals.md) before you can debug it on demand.
- The code must make at least one executable call; the debugger stops just before the first
  executable line.
- When the script finishes you can run it again or switch back to the editor.
- Edits you make inside the debugger are not saved anywhere. Copy them back into your IDE yourself.
- Output from [N/log](../modules/N_log.md) calls shows up on the debugger's Execution Log subtab.

## Debugging a deployed script

Deployed debugging works for script deployments and core plug-in implementations. Requirements:

- the deployment's status is **Testing**, and
- you are the script's owner.

In the script record, the button reads **Debug** in view mode and **Save & Debug** in edit mode.
**Debug Existing** lists only the Testing deployments for the API version you selected.

After you start, the debugger shows that it is waiting for a user action. Perform the action that
triggers the script, preferably in another browser tab or window; the session picks up from there.

- Scheduled scripts load without any user action.
- Sublists that belong to child records are available only when the parent allows child record
  editing. Read-only sublists are not supported, because the child record sublist is not loaded.
- A CSV import may not trigger debugging, for example when it runs under a different role or entity.
- Bulk processing runs and writes its logs without the debugger capturing it.

When the script finishes you can rerun it or switch to the editor. As with on-demand debugging,
edits made in the debugger are not saved: copy them back and upload the file again. Cancel a session
with the **X** button.

## Output and subtabs

Messages, alerts, errors and execution metrics, plus [N/log](../modules/N_log.md) output, appear on the
**Execution Log** subtab. The debugger has these subtabs:

- Execution Log
- Break Points
- Watch
- Evaluated Expressions

The [Script Debugger Interface](debugger.legacy_interface.md) page describes each area in detail.
