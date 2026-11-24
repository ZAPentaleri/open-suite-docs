# SuiteScript Unit Testing
<!-- id: topic:unit_testing -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

Sub-topics:

- [Mocking SuiteScript in Unit Tests](unit_testing.mocking.md)

A unit test checks one small piece of a script on its own: a line, a method or a function. For
SuiteScript the framework is **Jest**, run from Visual Studio Code with the SuiteCloud extension and the
**SuiteCloud Unit Testing** package. Tests use Jest matchers to state expectations and Jest mock
functions to stand in for SuiteScript modules.

## Setting up a project

1. Install the SuiteCloud extension for Visual Studio Code.
2. Create a SuiteCloud project.
3. Install SuiteCloud Unit Testing into it.

The project then contains:

| Item | Holds |
|---|---|
| `src` | The scripts under test |
| `__tests__` | The test files |
| `jest.config.js` | Jest settings for the project, including custom stub naming and file paths |
| `package.json` | Package settings; code coverage is switched on here |

Running the tests also produces `package-lock.json`, a `coverage` folder and `node_modules`. The
coverage folder appears only when coverage is configured in `jest.config.js`.

## Writing a test file

A test file imports the script under test and the SuiteScript modules it depends on. The module paths
resolve through the SuiteCloud Jest configuration, so the test can name modules such as `N/record`
the same way the script does.

Organize tests with `describe` groups, and write each test in three parts:

| Part | What goes in it |
|---|---|
| Given | Setup and mocks |
| When | The processing being tested |
| Then | The expectations |

Jest's lifecycle hooks `beforeAll`, `beforeEach`, `afterAll` and `afterEach` are available; clearing
mocks in `beforeEach` is a common use.

Recommendations:

- Base your tests on the script's conditional branches, so each path gets exercised.
- Track which lines the tests reach with code coverage, and aim for at least 80% coverage.

## Example

General Jest usage, not SuiteScript API: a test for a pure helper module that picks a freight band
from a weight and an injected rate lookup. The paths are illustrative.

```js
// __tests__/freightBand.test.js
const { freightBand } = require('../src/FileCabinet/SuiteScripts/lib/freightBand');

describe('freightBand', () => {
  const rateLookup = jest.fn();

  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('puts a 12 kg parcel in the standard band', () => {
    // Given
    rateLookup.mockReturnValue(7.25);
    // When
    const result = freightBand(12, rateLookup);
    // Then
    expect(result.band).toBe('standard');
    expect(rateLookup).toHaveBeenCalledTimes(1);
  });

  it('adds a surcharge above 30 kg', () => {
    rateLookup.mockReturnValue(7.25);
    const result = freightBand(42, rateLookup);
    expect(result.charge).toBeGreaterThan(7.25);
  });
});
```

For stubbing SuiteScript modules, records and entry point contexts, see
[Mocking SuiteScript in Unit Tests](unit_testing.mocking.md).
