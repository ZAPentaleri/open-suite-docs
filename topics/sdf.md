# SuiteCloud Development Framework
<!-- id: topic:sdf -->

Sub-topics:

- [SDF deploy file](sdf.deploy_file.md)
- [SDF manifest dependencies](sdf.manifest_dependencies.md)
- [SDF custom object files](sdf.object_files.md)
- [SDF script objects](sdf.script_objects.md)
- [SDF value lists for script objects](sdf.value_lists.md)
- [SDF translation collections](sdf.translation_collections.md)
- [Other SDF objects used with scripts](sdf.other_objects.md)
- [SDF toolset object](sdf.toolset.md)
- [SDF singlepageapp object](sdf.singlepageapp.md)
- [SDF bundleinstallationscript object](sdf.bundleinstallationscript.md)
- [SDF clientscript object](sdf.clientscript.md)
- [SDF eventsubscriber object](sdf.eventsubscriber.md)
- [SDF mapreducescript object](sdf.mapreducescript.md)
- [SDF massupdatescript object](sdf.massupdatescript.md)
- [SDF portlet object](sdf.portlet.md)
- [SDF restlet object](sdf.restlet.md)
- [SDF scheduledscript object](sdf.scheduledscript.md) (its own sub-topic: [SDF scheduled deployment recurrence](sdf.recurrence.md))
- [SDF installation script objects](sdf.sdfinstallationscript.md)
- [SDF suitelet object](sdf.suitelet.md)
- [SDF usereventscript object](sdf.usereventscript.md)
- [SDF workflowactionscript object](sdf.workflowactionscript.md)

See also: [Script record](script_record.md), [Script deployment](script_deployment.md),
[Script parameters](script_parameters.md).

## What SDF is

The SuiteCloud Development Framework (SDF) is the deployment layer between the SuiteCloud SDK
tools you run locally and a NetSuite account. A project describes pieces of the account as XML
definitions, and SDF applies those definitions to the account. Three kinds of component are
represented that way:

- **files**, which end up in the File Cabinet (your SuiteScript sources among them),
- **scripts**, that is, script records together with their deployments, and
- **custom objects**, such as custom records, fields, forms and roles.

Every object type SDF understands, with its XML root tag and the script ID prefix it uses, is in
[catalog:sdf_object_types](../gen/catalogs/sdf_object_types.md). How object files are named and how
values are written inside them is described in [SDF custom object files](sdf.object_files.md).

A note on naming: what is now called a SuiteCloud project used to be called an SDF project, so
older material may use that term for the same thing.

## What a deploy does to the account

Deploying replaces; it does not merge. Two overwrite rules follow from that:

- An object in the project overwrites the account object that carries the **same script ID**.
- A file in the project overwrites the File Cabinet file with the **same name**.

Validation compares the project with the target account and reports the differences between the
two, so you can see what a deploy would change before you run it. Which files and objects a deploy
sends, and in what order, is set in the [deploy file](sdf.deploy_file.md); references to things that
already exist in the account or in other SuiteApps are declared in the project manifest (see
[SDF manifest dependencies](sdf.manifest_dependencies.md)).

## Project types

There are two kinds of SDF project:

| Project type | Intended for |
|---|---|
| Account customization project (ACP) | Customizing your own accounts |
| SuiteApp project | SuiteCloud Developer Network (SDN) members who distribute SuiteApps |

Several rules differ between the two (for example, how deploys treat existing account objects and
what a project may refer to outside itself); the sub-topics point this out where it matters.

## What you can customize

The customization areas SDF supports are:

- lists, records and fields
- forms
- import and export
- dashboards and portlets
- scripting
- plug-ins
- centers and tabs
- users and roles
- reports
- web site management
- analytics workbooks
- AI

## Script objects at a glance

Each SuiteScript type that SDF can deploy has its own object type. The object combines the script
record and its deployments, and its fields are listed in generated catalogs (object fields,
deployment fields and, where the type has them, script parameter fields) linked from its sub-topic.

| Object | Script type page | Sub-topic |
|---|---|---|
| `bundleinstallationscript` | [BundleInstallationScript](../script-types/BundleInstallationScript.md) | [sdf.bundleinstallationscript](sdf.bundleinstallationscript.md) |
| `clientscript` | [ClientScript](../script-types/ClientScript.md) | [sdf.clientscript](sdf.clientscript.md) |
| `eventsubscriber` | [EventSubscriber](../script-types/EventSubscriber.md) | [sdf.eventsubscriber](sdf.eventsubscriber.md) |
| `mapreducescript` | [MapReduceScript](../script-types/MapReduceScript.md) | [sdf.mapreducescript](sdf.mapreducescript.md) |
| `massupdatescript` | [MassUpdateScript](../script-types/MassUpdateScript.md) | [sdf.massupdatescript](sdf.massupdatescript.md) |
| `portlet` | [Portlet](../script-types/Portlet.md) | [sdf.portlet](sdf.portlet.md) |
| `restlet` | [Restlet](../script-types/Restlet.md) | [sdf.restlet](sdf.restlet.md) |
| `scheduledscript` | [ScheduledScript](../script-types/ScheduledScript.md) | [sdf.scheduledscript](sdf.scheduledscript.md) |
| `sdfinstallationscript` | [SDFInstallationScript](../script-types/SDFInstallationScript.md) | [sdf.sdfinstallationscript](sdf.sdfinstallationscript.md) |
| `suitelet` | [Suitelet](../script-types/Suitelet.md) | [sdf.suitelet](sdf.suitelet.md) |
| `usereventscript` | [UserEventScript](../script-types/UserEventScript.md) | [sdf.usereventscript](sdf.usereventscript.md) |
| `workflowactionscript` | [WorkflowActionScript](../script-types/WorkflowActionScript.md) | [sdf.workflowactionscript](sdf.workflowactionscript.md) |

Rules shared by all of them (ID prefixes, defaults, audience fields, status values) are on
[SDF script objects](sdf.script_objects.md), and the enumerated values their fields accept are on
[SDF value lists for script objects](sdf.value_lists.md). Objects that support scripts without
being script records (custom tool sets, single page applications, integrations and secrets) are on
[Other SDF objects used with scripts](sdf.other_objects.md), [SDF toolset object](sdf.toolset.md) and
[SDF singlepageapp object](sdf.singlepageapp.md).
