# SuiteScript 2.1 script basics
<!-- id: topic:ss21_basics -->

Part of: [SuiteScript 2.1 overview](ss21_overview.md)

See also: [Global Objects](global_objects.md) (for [`define`](../globals.md) and
[`require`](../globals.md)), [SuiteScript 2.1 custom modules](custom_modules.md),
[SuiteScript 2.1 JSDoc validation](jsdoc_validation.md), [Record Modes](record_modes.md).

## Four patterns to know

2.1 scripts are built on four ideas: a modular architecture, entry points, context objects, and
methods that take a single options object as their argument. Background reading that helps: the
SuiteScript user group, NetSuite's training courses, the AMD (Asynchronous Module Definition)
specification and RequireJS.

## What "a script" means

- **Script:** the entry point script file, any custom modules it uses, and the script record that
  registers it.
- **Script deployment:** the record whose settings decide how the script behaves at run time, such
  as the record types it applies to, when it executes, its audience and its log level.

## Anatomy of an entry point script

A file contains, in order:

1. JSDoc tags: `@NApiVersion` and `@NScriptType`.
2. A call to [`define`](../globals.md) listing the dependencies.
3. The callback that `define` invokes once those dependencies are loaded.
4. The entry point functions.
5. A `return` statement exposing the entry points.

Rules:

- `@NApiVersion` accepts `2.0`, `2.1`, `2.x` and `2.X`.
- `@NScriptType` is not case-sensitive.
- One entry point script belongs to exactly one script type, and it must expose at least one entry
  point of that type. It needs both the entry point and the function that implements it.
- Entry points belong to their script type: an entry point of one type cannot be used in a script
  of another type.
- The callback's parameters line up with the dependency list by position: the first parameter
  receives the first module, and so on.
- Dependencies are loaded when the script runs.

## Modules, define and require

- 2.1 uses the AMD module system. Modules are created with [`define`](../globals.md), which takes
  between one and three arguments; the usual pair is the dependency array and the callback.
- The callback of an entry point script must return at least one entry point.
- [`require`](../globals.md) is only meant for loading modules on demand while debugging.
- Code run in the debugger or a browser console does not have to be wrapped as a module.

## Context objects

- When NetSuite calls an entry point, the system supplies the context object. When a custom module
  function is called, the calling script supplies its arguments.
- Which properties a context object has depends on the entry point.

## Options objects

Most 2.x methods take one object of named options instead of positional arguments; for example
[`Record.setValue`](../modules/N_record.md) takes `fieldId` and `value`.

- The object can have any variable name, or be written inline in the call.
- Do not put inline comments inside an object literal; they are not supported and may fail without
  any error.

## Reserved words

- Names that are ECMAScript 2015 reserved words, such as `var` or `try`, cannot be used for
  variables or functions; the script fails with a syntax error.
- Avoid future reserved words as well.
- A 1.0 script that used `log` or `util` as its own variable names clashes with the 2.x global
  objects of the same names.

## Usage units

When a script uses up its usage units, it is stopped with an error. See [Governance](governance.md).

## Building a script, step by step

1. Define the dependencies.
2. Add the JSDoc tags.
3. Write the entry point function.
4. Move supporting code into custom modules.
5. Upload the file and deploy it.

## Terms

| Term | Meaning |
|---|---|
| Class | A template that bundles data with the code that works on it; available in 2.1 only |
| Constants | Named references to fixed values; available in 2.1 only |
| Custom module | A library you write yourself and load as a dependency |
| Destructuring | Unpacking arrays, objects or maps into separate variables; available in 2.1 only |
| Entry point | The place where the system hands control to your script |
| Entry point function | The function that runs when its entry point is invoked |
| Entry point script | The primary file attached to the script record |
| Enum | A plain object whose values are read-only strings, such as [`record.Type`](../modules/N_record.md) |
| Governance | The budget of usage units a script may spend in one execution |
| Usage units | See Governance |
| Script | The entry point script, its custom modules and the script record together |
| Script deployment | The record whose settings control how the script behaves at run time |
| Script type | The kind of script, each meant for particular tasks and triggers |
| Standard mode | Record mode where sourcing, calculation and validation happen when the record is saved |
| Dynamic mode | Record mode where sourcing, calculation and validation happen in real time, imitating how a record behaves in the UI |
| Deferred dynamic mode | Another name for standard mode |

For record modes in detail see [Record Modes](record_modes.md); for custom modules see
[SuiteScript 2.1 custom modules](custom_modules.md).

## Example

A custom module and the scheduled script that uses it. The module has only `@NApiVersion`; the
entry point script adds `@NScriptType`, lists its dependencies in the same order as the callback
parameters, and returns its single entry point.

```js
/**
 * @NApiVersion 2.1
 */
define([], () => {
  const DAY_MS = 24 * 60 * 60 * 1000;
  const daysLate = (dueDate, today = new Date()) =>
    Math.max(0, Math.floor((today.getTime() - dueDate.getTime()) / DAY_MS));
  return { daysLate };
});
```

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/format', 'N/log', './taskAging'], (search, format, log, taskAging) => {
  const execute = () => {
    const lateTasks = search.create({
      type: 'task',
      filters: [['duedate', 'before', 'today']],
      columns: ['title', 'duedate']
    });
    lateTasks.run().getRange({ start: 0, end: 50 }).forEach((row) => {
      const due = format.parse({ value: row.getValue({ name: 'duedate' }), type: format.Type.DATE });
      log.debug({
        title: row.getValue({ name: 'title' }),
        details: `${taskAging.daysLate(due)} days late`
      });
    });
  };

  return { execute };
});
```
