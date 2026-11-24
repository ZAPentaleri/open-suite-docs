# Entity records
<!-- id: topic:records_guide.entities -->

Part of: [SuiteScript records guide](records_guide.md)

Entity records represent the people and organizations an account deals with, such as
[employees](../gen/records/employee.md), [customers](../gen/records/customer.md) and
[vendors](../gen/records/vendor.md).

## Restrictions around administrator access

To protect administrator accounts, scripts are blocked from touching the most privileged entities.
A script cannot:

- create an entity that holds the Administrator role or a role with Full access,
- delete such an entity,
- give the Administrator role to an entity, or take it away, or
- change the password or email address of an entity that holds the Administrator role or a
  Full-access role.

These restrictions apply to every SuiteScript version and every kind of account. A script that
attempts any of these operations fails with a script security violation error. The spec does not
give the exact error code, so catch the error generically and log it rather than matching on a
specific code.

## Example

A scheduled script that updates employee emails in bulk and logs, rather than aborts on, the
updates the platform refuses (for example on an administrator). The employee IDs and addresses are
illustrative.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/record', 'N/log'], (record, log) => {
  const MOVES = [
    { id: 311, email: 'r.ortega@example.invalid' },
    { id: 47, email: 'k.ngata@example.invalid' }
  ];

  const execute = () => {
    MOVES.forEach(({ id, email }) => {
      try {
        record.submitFields({
          type: record.Type.EMPLOYEE,
          id,
          values: { email }
        });
      } catch (e) {
        log.error({ title: `Employee ${id} not updated`, details: `${e.name}: ${e.message}` });
      }
    });
  };

  return { execute };
});
```
