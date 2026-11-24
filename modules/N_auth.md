# N/auth
<!-- id: N/auth -->

`N/auth` lets a server script change a user's login credentials: the email address that serves as
the login name, or the password. Both calls take a password as input, so they fit self-service
flows where the user types the values in; never store such values in a script or its parameters.

- **Availability:** server scripts only.
- **Load it as:** `define(['N/auth'], (auth) => { ... })`

## Members

| Member | Kind | Summary |
|---|---|---|
| [`auth.changeEmail(options)`](#authchangeemailoptions) | function | Change the user's login email |
| [`auth.changePassword(options)`](#authchangepasswordoptions) | function | Change the user's password |

## Functions

### auth.changeEmail(options)
<!-- id: N/auth.changeEmail -->

Changes the email address the user logs in with. By default the change applies only to the user's
roles in the current account; pass `onlyThisAccount: false` to apply it to all of the user's
accounts and roles.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.onlyThisAccount` | boolean | No | `true` |
| `options.newEmail` | string | Yes | |
| `options.password` | string | Yes | |

- `options.onlyThisAccount`: `true` limits the change to the roles in the current account; `false`
  changes the login for all accounts and roles.
- `options.newEmail`: the new login email.
- `options.password`: the user's password, as entered by the user.

**Returns:** `void`

**Errors:**

- `WRONG_PARAMETER_TYPE`: `options.onlyThisAccount` has the wrong type; pass a real boolean.
- `INVALID_EMAIL`: `options.newEmail` is not a valid email address.
- `INVALID_PSWD`: `options.password` fails authentication or breaks the password rules.
- `SSS_MISSING_REQD_PARAMETER`: `options.newEmail` or `options.password` is missing.

> [!SOURCE-ERROR] For a missing argument the source names `SSS_MISSING_REQD_PARAMETER` here but
> `SSS_MISSING_REQD_ARGUMENT` for `changePassword`. The source does not say which code the runtime
> actually raises; check for both when handling a missing value.
<!-- source-error: N/auth.changeEmail | - | missing_param_code_reqd_parameter_vs_reqd_argument -->

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

### auth.changePassword(options)
<!-- id: N/auth.changePassword -->

Changes the user's password. The new password must satisfy the account's password rules.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.currentPassword` | string | Yes | |
| `options.newPassword` | string | Yes | |

**Returns:** `void`

**Errors:**

- `INVALID_PSWD`: authentication fails, or the new password breaks the password rules.
- `INVALID_EMAIL`: listed by the source for an invalid argument value, although this function takes
  no email (see the note below).
- `SSS_MISSING_REQD_ARGUMENT`: `options.currentPassword` or `options.newPassword` is missing.

> [!SOURCE-ERROR] The source's error rows for this function appear to have been copied from
> `changeEmail`, hence an `INVALID_EMAIL` entry for a password-only call. Expect `INVALID_PSWD` and
> the missing-argument error; do not count on the email error occurring here.
<!-- source-error: N/auth.changePassword | - | error_rows_copied_from_changeemail -->

**Governance:** 10 units.

**Availability:** server scripts.

**Since:** 2015.2

## Examples

A Suitelet behind a partner self-service page: the partner submits a form and the script changes
their login email for this account only, reporting a friendly message on failure. Every credential
value comes from the submitted form, never from the code.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/auth'], (auth) => {
  const onRequest = (context) => {
    const { newEmail, confirmPassword } = context.request.parameters;
    if (!newEmail || !confirmPassword) {
      context.response.write('Enter the new address and your password.');
      return;
    }
    try {
      auth.changeEmail({ newEmail, password: confirmPassword, onlyThisAccount: true });
      context.response.write(`Your login is now ${newEmail}.`);
    } catch (e) {
      const reason = e.name === 'INVALID_EMAIL' ? 'That address is not valid.' : 'Password check failed.';
      context.response.write(reason);
    }
  };

  return { onRequest };
});
```

The same pattern for a password change, handing the error code back to the caller:

```js
/**
 * @NApiVersion 2.1
 */
define(['N/auth'], (auth) => {
  const rotatePassword = (formValues) => {
    try {
      auth.changePassword({
        currentPassword: formValues.oldPwd,
        newPassword: formValues.newPwd
      });
      return { ok: true };
    } catch (e) {
      return { ok: false, code: e.name };
    }
  };

  return { rotatePassword };
});
```
