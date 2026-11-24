# Setting Runtime Options
<!-- id: topic:runtime_options -->

Part of: [Script Deployment](script_deployment.md)

See also: [N/log](../modules/N_log.md), [N/recordContext](../modules/N_recordContext.md),
[N/query](../modules/N_query.md), [The Scripted Records Page](monitoring.scripted_records.md),
[SuiteScript Debugger](debugger.md), catalog
[localization_context_records](../gen/catalogs/localization_context_records.md).

The script deployment record lets you change how, when and for whom a script runs without touching
the script file. The runtime options are:

- event type,
- log level,
- execute as role,
- available without login,
- deployment status,
- audience, and
- script parameters (see [Creating Script Parameters](script_parameters.md)).

Each section below says which script types the option applies to.

## Event type

The Event Type field is offered for Suitelets, user event scripts and record-level client scripts.

An event type set on the deployment takes precedence over the event types the script handles in its
code. Left blank, the script runs for whichever events its file handles. For example, a script written
for create whose deployment is set to edit fires only on edit. This lets you change the triggering
event without editing the JavaScript file.

## Log level

Set in the deployment's **Log Level** field. It filters which entries appear on the deployment's
Execution Log subtab:

| Level | Entries kept |
|---|---|
| Debug | All entries |
| Audit | Audit, Error and Emergency |
| Error | Error and Emergency |
| Emergency | Emergency only |

Use Debug while testing and Audit or a stricter level in production; Audit fits events such as a
request sent to an external site. The log level is unrelated to how the script handles errors. The
amount of logging is governed per 60-minute window (see
[governance](governance.md#logging-limits)). Log entries themselves are written with
[N/log](../modules/N_log.md).

## Execute as role

Available for Suitelets, user event scripts, portlets, mass update scripts and workflow action
scripts, in the deployment's **Execute As Role** field (Script Deployments, Edit).

The chosen role is applied when the user running the script does not have the access the script needs.
You can choose any standard or custom role, or **Current User**. Typical reasons:

- the script must reach records the user cannot see (for instance, a script on sales orders that
  creates a follow-up task and needs employee records);
- bundle installation scripts, which must run as Administrator;
- scripts without a session that still need to read data.

Keep in mind:

- Use Administrator sparingly; where possible, create a custom role with exactly the permissions the
  script needs.
- A script triggered by another script runs with the triggering script's role. A script set to run as a
  sales person role therefore runs as that role even when an administrator triggers it.
- Leave the deployment in Testing status while you are trying out the role.

## Available without login

Suitelets only. It lets people who are not logged in run the Suitelet, which suits one-off online
forms, inbound notifications from partners, unsubscribe requests, and mashups on third-party sites
that post data into NetSuite. Data handled this way is less secure, since no login protects it.

Requirements:

- the Online Form User external role, which needs the Website and Online Forms features;
- the departments, groups, employees and partners on the audience must be cleared;
- every role that needs access must be selected in both the internal and the external roles.

Online Forms is switched on under Enable Features, CRM subtab, Marketing section. If the Online Form
User role is still missing after enabling it, log out and back in.

Do not use this option for integrations; doing so also breaks the Built for NetSuite standards.

## Deployment status

| Status | Who the script runs for |
|---|---|
| Testing | The owner and the audience members acting as testers |
| Released | The whole audience |

The Testing and Released statuses are not available for scheduled scripts.

- Debugging a deployed script requires Testing; a Released deployment cannot be debugged.
- The owner can test as another role by putting that role on the **Audience** subtab.
- A bundle installation script cannot run in the target account while it is in Testing. When Released,
  it runs on bundle install or update.
- With an empty audience the script runs only for its owner, even when Released.

## Audience

Available for Suitelets, portlets, user event scripts, record-level client scripts, RESTlets, mass
update scripts and workflow action scripts. Not available for scheduled, map/reduce and bundle
installation scripts.

- An empty audience means the owner only, even when Released.
- If both roles and departments are set, a user must match both.
- Groups, employees and partners are "any match": belonging to any one of them is enough.
- In OneWorld accounts the **Subsidiaries** field narrows it further: the script runs when the user's
  subsidiary is selected. In Testing status it runs for the owner and the specified audience.
- Keep the audience of a mass update and the audience of its deployment in step.
- To run for everybody, select all internal roles and all external roles. Otherwise grant only roles
  with a real need.
- While testing, switch to a role in the audience to see what its users get.

## Context filtering

The deployment's **Context Filtering** subtab filters client scripts and user event scripts by:

- **Execution context.** By default every context is selected except Web Application and Web Store.
  When the current context is not selected, the script is skipped.
- **Localization context.** Based on the subsidiary's country and on tax nexus; all countries by
  default.

Skipping contexts the script has no use for improves performance. See also
[N/recordContext](../modules/N_recordContext.md).

### Setting a localization context

1. Clear **Select All**.
2. Open the localization context pop-up. It pages the countries by first letter and offers a search.
3. Pick the countries; they are listed under the current selections.
4. Click **Done**.

Client scripts need both the subtab setting and the `localizationContextEnter` and
`localizationContextExit` entry points (see [ClientScript](../script-types/ClientScript.md)). User event
scripts are localized through the subtab alone.

A record can have at most 10 client scripts, counting localized and non-localized ones together. The
order of localized scripts is set on the [Scripted Records page](monitoring.scripted_records.md), and
localized scripts can be found with a query through [N/query](../modules/N_query.md).

The record types that support localization context are listed in
[catalog:localization_context_records](../gen/catalogs/localization_context_records.md).

> [!SOURCE-ERROR] In the source's list of records that support localization context, the tax code
> record appears twice, both times with the sales tax item ID. Do not assume that ID is right for tax
> codes; check the record type in your account.
<!-- source-error: topic:runtime_options | - | tax_code_listed_as_salestaxitem_twice -->
