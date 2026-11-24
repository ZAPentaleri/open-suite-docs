# SuiteScript 1.0 Suitelets
<!-- id: topic:ss1_api.suitelets -->

> **Legacy material.** This page exists so that 1.0 scripts can be read, converted and minimally
> repaired. Do not write new 1.0 code; the 2.1 counterpart is [Suitelet](../script-types/Suitelet.md).

Part of: [SuiteScript 1.0 API conventions](ss1_api.md)

## Entry function arguments

| Position | Argument | Object |
|---|---|---|
| 1 | `request` | [`nlobjRequest`](../ss1/objects-services.md) |
| 2 | `response` | [`nlobjResponse`](../ss1/objects-services.md) |

## What a Suitelet can send back

- Plain text, HTML, RSS or XML written to the response.
- A redirect to another NetSuite page.
- A redirect to an external URL. Only Suitelets that are externally available may do this.

## Back-end Suitelets

A back-end Suitelet has no UI of its own; it only takes a request and returns a response, for
example to serve data to a client script. Build its URL with
[`nlapiResolveURL`](../ss1/ui-http.md) rather than hard-coding it.

Do not route work through back-end Suitelets to get around governance limits. NetSuite treats that
as abuse.

## Reserved URL parameters

NetSuite uses these URL parameter names itself, so a Suitelet must not use them for its own data:

`e`, `id`, `cp`, `l`, `popup`, `s`, `d`, `_nodrop`, `sc`, `sticky`, `print`, `email`, `q`, `si`,
`st`, `r`, `displayonly`, `nodisplay`, `deploy`, `script`

A typical symptom of using one is the page reporting that there are no records of this type.
Prefix your own parameters with `custom` to stay clear of the reserved names.

## Converting

In 2.1 the function becomes the `onRequest` entry point of [Suitelet](../script-types/Suitelet.md),
and the request and response arrive on its context object. See
[Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md).

## Example (1.0, for reading legacy code)

A back-end Suitelet that returns, as plain text, how many tasks assigned to an employee are due
today or later. It reads its input from a `custom`-prefixed parameter.

```js
function upcomingTaskCount(request, response) {
  var assignee = request.getParameter('custom_assignee');
  if (!assignee) {
    response.write('0');
    return;
  }
  var filters = [
    new nlobjSearchFilter('assigned', null, 'anyof', assignee),
    new nlobjSearchFilter('duedate', null, 'onorafter', 'today')
  ];
  var rows = nlapiSearchRecord('task', null, filters, null);
  response.write(String(rows ? rows.length : 0));
}
```
