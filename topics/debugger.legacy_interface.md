# Script Debugger Interface
<!-- id: topic:debugger.legacy_interface -->

Part of: [Legacy debugger for 1.0 and 2.0](debugger.legacy.md)

This page walks through the screen of the legacy Script Debugger (**Customization > Scripting >
Script Debugger**). None of it applies to SuiteScript 2.1: for 2.1 scripts, logs, variables and
watches live in Chrome DevTools (see [Debugging SuiteScript 2.1 Scripts](debugger.ss2_1.md)).

The screen has three parts: the script area, the buttons, and the subtabs. An **API version**
dropdown chooses which runtime the code is debugged in.

## Buttons

| Button | Shortcut key | What it does |
|---|---|---|
| Step Over | Space | Run the current line and stop at the next one |
| Step Into | `i` | Enter the function being called and stop inside it |
| Step Out | `o` | Run until control returns to the caller, or until a breakpoint is hit |
| Continue | Shift+Space | Run until the next breakpoint |
| Abort / Cancel | `q` | Stop execution |

**Debug Script** pauses before the first line of code. While you change breakpoints during a run, a
"running script" indicator is shown.

## Subtabs

The full set of subtabs is Execution Log, Local Variables, Watches, Evaluate Expressions and Break
Points. (Elsewhere the source lists only the first three, or names them slightly differently; the
areas are the same.)

### Execution Log

- Entries are written by `log.debug` and the other [N/log](../modules/N_log.md) calls in the script.
- Columns: type, subject, details, timestamp. Timestamps are shown in your own time zone.
- Details are collapsed until you expand them.
- The log is cleared when a debug session starts.
- Logs produced while debugging are **not** written to the deployment's execution logs.

### Local Variables

Shows the local variables and the call stack. Display limits apply: at most 1,000 variables, and
string values are cut at 500 characters. To see the whole value of a large variable, add a watch on
it.

Properties of NetSuite objects are private; read them through the object's getter methods.

### Watches

- Watches are evaluated in the call-stack frame you have selected.
- Type an expression into the add-watch box and press Enter to add it; click the red X to remove it.
- You can watch a private property with dot notation, for example `record.id`.

### Evaluate Expressions

Expressions are evaluated in the selected call-stack frame. Evaluating an expression can change the
state of the running program, so be careful with anything that assigns or calls.

### Break Points

There are two kinds of breakpoint:

- **Line breakpoints.** Click in the gap between the line number and the code to add one; click the
  breakpoint icon or its red X to remove it. You can remove breakpoints whenever execution is paused.
- **User event breakpoints.** Stop when a given user event fires. The allowed events are offered in the
  **Break on User Event** list. These are available only when you debug a deployed script.

## A walkthrough

The source illustrates the interface with a short session that sets breakpoints, steps over, steps
into a function, and adds watches.

> [!SOURCE-ERROR] The source's walkthrough names one line number for the second breakpoint and later
> refers to it by a different number. Do not rely on its line numbers; follow the steps by what each
> line does.
<!-- source-error: topic:debugger.legacy_interface | - | second_breakpoint_line_given_as_18_then_15 -->
