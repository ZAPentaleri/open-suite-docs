# Other SDF objects used with scripts
<!-- id: topic:sdf.other_objects -->

Part of: [SuiteCloud Development Framework](sdf.md)

See also: [SDF toolset object](sdf.toolset.md), [SDF singlepageapp object](sdf.singlepageapp.md),
[CustomTool](../script-types/CustomTool.md), [Custom tool management](custom_tool.management.md),
[Single page applications](single_page_applications.md),
[catalog:sdf_object_types](../gen/catalogs/sdf_object_types.md).

Apart from the script objects, a handful of SDF object types exist mainly to support scripts:
custom tool sets, single page applications, integrations and secrets. The tool set and the single
page application have their own pages with full element listings; this page summarizes them and
covers integrations and secrets.

## Custom tools: `toolset` (and the retired `tool`)

Custom tool scripts (see [CustomTool](../script-types/CustomTool.md)) are packaged with the
`toolset` object; its elements are on [SDF toolset object](sdf.toolset.md).

The older `tool` object is deprecated. It is marked deprecated as of release 2027.1, and from
September 2026 it can no longer be deployed, so move projects over to `toolset`.

Permissions on a tool decide whether users can see the tool. They do not control data access, so do
not count on them to limit which records the tool can reach.

## Single page applications: `singlepageapp`

Details are on [SDF singlepageapp object](sdf.singlepageapp.md). The rules that matter when you lay
out the project:

- The object's script ID must match its file name.
- The application folder depends on the project type:

  | Project type | Folder |
  |---|---|
  | SuiteApp project | `SuiteApps/<appid>/` |
  | Account customization project | `SuiteScripts/<folder>/` |

- Paths inside the object are relative to that File Cabinet folder.
- The application's client-side and server-side scripts must both live inside the SPA folder.

## Integrations: `integration`

The `integration` object does not define an integration record. It holds no fields; it only points
at an existing integration record by script ID.

| Attribute | Type | Required | Notes |
|---|---|---|---|
| `scriptid` | string | Yes | At most 99 characters; the default value given is `custinteg` |

The source lists no other attributes or fields for this object.

- SDF cannot create an integration record.
- If the integration record already exists in the target account, the object is skipped and no
  error is raised.
- When the object is part of a SuiteApp, installing the SuiteApp installs the integration record
  and uninstalling the SuiteApp removes it.

```xml
<integration scriptid="custinteg_harborline_edi"/>
```

## Secrets: `secret`

A `secret` object stores a secret value that scripts can use without the value appearing in their
source code.

| Attribute | Type | Required | Notes |
|---|---|---|---|
| `scriptid` | string | Yes | At most 99 characters; the default value given is `custsecret` |

The source lists no other attributes or fields for this object, so it does not show how the secret
value itself is written in the project.

| Rule | Detail |
|---|---|
| Script ID prefix | `custsecret_` |
| Maximum length of the secret value | 1,000,000 characters |
| How scripts refer to it | Only by its script ID, for example `custsecret_harborline_sftp` |
| Who can use it | SuiteScript 2.x scripts only |
| SuiteApps per secret | At most one SuiteApp can be associated with a secret |
| Where it can be updated | Only in the account in which it was created |

Never put a real secret value into a project file you share; scripts reference the secret only by
its script ID.
