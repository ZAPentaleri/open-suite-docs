# SDF deploy file
<!-- id: topic:sdf.deploy_file -->

Part of: [SuiteCloud Development Framework](sdf.md)

See also: [SDF manifest dependencies](sdf.manifest_dependencies.md),
[SDF installation script objects](sdf.sdfinstallationscript.md),
[SDF translation collections](sdf.translation_collections.md).

Every SDF project carries a deploy file called `deploy.xml`. It decides which parts of the project
are pushed to the account and in which order.

## Structure

The root element is `deploy`. Inside it are three sections, and each of them is nothing more than
a list of `path` elements:

| Section | What its paths point at |
|---|---|
| `configuration` | Account configuration files |
| `files` | File Cabinet content, such as your SuiteScript sources |
| `objects` | Custom object XML files |

If you leave the file as generated, it holds these default paths:

- `~/FileCabinet/SuiteScripts/*`
- `~/Objects/*`
- `~/AccountConfiguration/*` (added only in account customization projects)

An illustrative deploy file for an account customization project (folder and object names are
invented):

```xml
<deploy>
  <configuration>
    <path>~/AccountConfiguration/*</path>
  </configuration>
  <files>
    <path>~/FileCabinet/SuiteScripts/northwind_common/*</path>
    <path>~/FileCabinet/SuiteScripts/northwind_freight/*</path>
  </files>
  <objects>
    <path>~/Objects/customrecord_nw_carrier_zone.xml</path>
  </objects>
  <objects>
    <path>~/Objects/*</path>
  </objects>
</deploy>
```

> [!SOURCE-ERROR] One deploy file example in the source ends its `files` section with a
> self-closing slash rather than a real `</files>` end tag. Close the section with a normal end tag,
> as above, and do not copy the self-closing form.
<!-- source-error: topic:sdf.deploy_file | - | example_closes_files_with_self_closing_tag -->
<!-- source-error: topic:sdf.deploy_file | - | example_closes_files_with_self_closing_tag -->

## How paths are written and processed

- Each path is anchored at the project root, which is written as `~`.
- Path names are case-sensitive, so `~/Objects/CustList_Region.xml` and
  `~/Objects/custlist_region.xml` are two different paths.
- A trailing `*` selects every file in that folder; for the files a wildcard expands to, SDF works
  out the order itself.
- Paths are handled in the order they appear, so put dependencies first. In the example above the
  shared library folder precedes the folder whose scripts use it, and the custom record comes before
  the wildcard that brings in objects referring to it.
- You may repeat the `files` or `objects` section to control ordering further, as the two `objects`
  blocks above do.

## What gets deployed

Only what a path selects is deployed. Project files that no path covers are still uploaded with the
project, but they are not deployed to the account. The zipped project may be at most 50 MB.

In an account customization project, the objects you deploy always replace their counterparts in the
account.

## References outside the project

- **SuiteApp projects** may refer only to components that are part of the project or that belong to
  a SuiteApp already installed in the account.
- **Account customization projects** may refer to components that already exist in the account, as
  long as they are declared as dependencies in the project manifest. The manifest's element layout
  is described in [SDF manifest dependencies](sdf.manifest_dependencies.md).

## Running installation scripts

The deploy file can also carry `run` elements, which are used for SDF installation scripts (see
[SDF installation script objects](sdf.sdfinstallationscript.md)). A `run` element holds a `script`
element, and that holds two children:

| Element | Holds |
|---|---|
| `deploy` > `run` > `script` > `path` | The path of the installation script object file, for example `~/Objects/customscript_nw_seed_zones.xml` |
| `deploy` > `run` > `script` > `deployment` | The script ID of the deployment to run, for example `customdeploy_nw_seed_zones` |

Where you place the `run` element among the other entries decides when the installation script
runs during the deploy: put it after the objects the script depends on, or before objects it should
prepare for.

```xml
<deploy>
  <files>
    <path>~/FileCabinet/SuiteScripts/northwind_freight/*</path>
  </files>
  <objects>
    <path>~/Objects/customrecord_nw_carrier_zone.xml</path>
    <path>~/Objects/customscript_nw_seed_zones.xml</path>
  </objects>
  <run>
    <script>
      <path>~/Objects/customscript_nw_seed_zones.xml</path>
      <deployment>customdeploy_nw_seed_zones</deployment>
    </script>
  </run>
  <objects>
    <path>~/Objects/*</path>
  </objects>
</deploy>
```

Here the installation script runs once the custom record and the script object itself are in place,
and before the remaining objects are deployed.
