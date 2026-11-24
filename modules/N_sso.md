# N/sso
<!-- id: N/sso -->

`N/sso` is a retired module. Support for it ended with release 2025.1, and a script that still uses
it now fails with an error when it runs. The spec lists no members for it, so there is nothing left
to call.

- **Status:** no longer supported (support ended in 2025.1).
- **What happens if a script uses it:** the script raises an error at run time. The source does
  not state the error code.
- **What to do:** remove `N/sso` from the `define` dependency list of any script that still names
  it, together with the code that used it.
- **Replacement:** the source does not name a replacement module.

## Examples

Cleaning up a legacy user event script that still listed the module. Only the dependency list and
the binding change; the remaining logic stays as it was.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
// Before: define(['N/sso', 'N/runtime'], (sso, runtime) => { ... })
// After: the retired module is gone from the dependency list.
define(['N/runtime'], (runtime) => {
  const beforeLoad = (context) => {
    const user = runtime.getCurrentUser();
    log.debug({ title: 'Form opened', details: `${context.type} by user ${user.id}` });
  };

  return { beforeLoad };
});
```
