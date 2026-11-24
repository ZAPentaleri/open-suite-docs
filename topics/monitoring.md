# Script monitoring and logs
<!-- id: topic:monitoring -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

Sub-topics:
- [The Scripted Records Page](monitoring.scripted_records.md)

See also: [Governance and limits](governance.md) (logging limits), [Setting runtime options](runtime_options.md)
(log levels), [N/log](../modules/N_log.md).

## Where script logs appear

| Place | What you see |
|---|---|
| Script record page | Execution Log subtab for all of the script's deployments |
| Script deployment page | Execution Log subtab for that deployment; the script must be deployed for this to show anything |
| SuiteScript Debugger | Execution Log subtab for the debug session; debugger logs appear **only** here |
| Script Execution Logs page (**Customization > Scripting > Script Execution Logs**) | Logs across scripts, with filters |

Custom tools have no script or deployment pages, so their logs are only on the Script Execution Logs
page.

Typical entries are a system error (for instance an unexpected error from calling a method that is
not defined) or a user entry written with `log.debug`.

### The Execution Log subtab

- Times are shown in the user's preferred time zone.
- By default it shows the current day and every log type. Use **Customize View** to search other days
  or by other criteria.

### The Script Execution Logs page

- Keeps 30 days of logs.
- Times follow the user's time zone preference.
- You can filter, download (CSV or Excel, up to 10,000 entries) and print.
- A filter example: web services and RESTlet concurrency violations.
- Logs are not carried over when an account is moved to an OCI data center.

## Outbound request log

The **Outbound Requests** list (**Setup > Company > Communication > Outbound Request**) logs the
outbound HTTPS and SFTP calls that scripts make.

- Administrators see it by default; other roles need the **Outbound Request** permission, a list-type
  permission.
- Each entry shows the URL, the method and the result. For SFTP, it links to the deployment that made
  the request.
- You can customize the list view and build ad hoc and saved searches over outbound requests.
- Use it to audit account activity and to find requests that fail or are inefficient.

## Application Performance Management

For a wider look at how customizations perform, use the Application Performance Management (APM)
SuiteApp.
