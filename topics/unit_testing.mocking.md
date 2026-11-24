# Mocking SuiteScript in Unit Tests
<!-- id: topic:unit_testing.mocking -->

Part of: [SuiteScript Unit Testing](unit_testing.md)

See also: [N/record](../modules/N_record.md), [N/currentRecord](../modules/N_currentRecord.md),
[UserEventScript](../script-types/UserEventScript.md), [Suitelet](../script-types/Suitelet.md).

SuiteScript modules do not exist outside NetSuite, so a unit test replaces everything the script
touches with mocks. Every module member a script uses can be mocked. This page covers methods and
objects, properties, enums, entry points and conditional statements.

## Methods and objects

Replace methods with Jest mock functions, and have them return what the scenario needs (for example
with `mockReturnValue`). The source illustrates this with module methods returning stand-in objects.

## Properties

Mock a property by assigning it a value of a valid type. For example,
[`Record.type`](../modules/N_record.md) can simply be set to a string.

## Enums

Define the enum inside the test, with the exact values the script uses. You do not need every value
of the real enum, only the ones the code reads. If the values do not match what the script expects,
the test fails with an error.

## Entry points

To call an entry point, build a context object holding the parameters that entry point receives and
pass it in. The source shows this for a user event `beforeLoad` and for a Suitelet.

## Conditional statements

Test both branches of every condition. The source's example is a client script `validateLine` that
branches on the sublist ID and reads values with
[`CurrentRecord.getSublistValue`](../modules/N_currentRecord.md).

## Example

The script under test sets a default memo on new sales orders:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define([], () => {
  const beforeLoad = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    if (context.newRecord.type === 'salesorder') {
      context.newRecord.setValue({ fieldId: 'memo', value: 'Web order: confirm ship date' });
    }
  };

  return { beforeLoad };
});
```

A Jest test that mocks the context, the record property and the enum (general Jest usage; the path
is illustrative):

```js
// __tests__/so_default_memo.test.js
const script = require('../src/FileCabinet/SuiteScripts/so_default_memo');

describe('beforeLoad default memo', () => {
  // Only the enum values the script reads; placeholder strings, used consistently.
  const UserEventType = { CREATE: 'ue-create', VIEW: 'ue-view' };
  let newRecord;

  beforeEach(() => {
    newRecord = { type: 'salesorder', setValue: jest.fn() };
  });

  it('sets the memo when a sales order is created', () => {
    // Given
    const context = { type: UserEventType.CREATE, UserEventType, newRecord };
    // When
    script.beforeLoad(context);
    // Then
    expect(newRecord.setValue).toHaveBeenCalledWith({
      fieldId: 'memo',
      value: 'Web order: confirm ship date'
    });
  });

  it('leaves the record alone when it is only viewed', () => {
    const context = { type: UserEventType.VIEW, UserEventType, newRecord };
    script.beforeLoad(context);
    expect(newRecord.setValue).not.toHaveBeenCalled();
  });
});
```

The test file relies on the SuiteCloud Jest configuration to load the AMD script; see
[SuiteScript Unit Testing](unit_testing.md).
