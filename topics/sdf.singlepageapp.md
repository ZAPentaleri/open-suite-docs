# SDF singlepageapp object
<!-- id: topic:sdf.singlepageapp -->

Part of: [SuiteCloud Development Framework](sdf.md)

Related: [Single Page Applications](single_page_applications.md), [SDF value lists for script
objects](sdf.value_lists.md), [SDF object types](../gen/catalogs/sdf_object_types.md).

A `singlepageapp` object is what you put in an SDF project to define a
[single page application](single_page_applications.md) (SPA). The object ties together the app's
name and URL, the script that runs in the browser and the one that runs on the server, the File
Cabinet folders that hold code and assets, the audience, the execution role, logging, notifications
and the places in the UI where links to the app appear. There is no generated element catalog for
this object type; the facts below come from the topic itself.

Before such an object can be deployed, the project manifest must declare the `SERVERSIDESCRIPTING`
feature as a dependency (see [SDF manifest dependencies](sdf.manifest_dependencies.md)).

## Identity

- The object has one XML attribute, `scriptid`, and it is required.
- Script IDs for this type begin with `custspa`, which is also the value the field starts out with.
  The [object types catalog](../gen/catalogs/sdf_object_types.md) lists the type with the prefix
  `custspa_`.
- A script ID can be at most 28 characters long, may contain only lower-case letters, digits and
  underscores, and must be unique. It is also the file name of the object's XML file.

## Elements

| Element | Type | Use | Notes |
|---|---|---|---|
| `scriptid` | string | Required | XML attribute; prefix `custspa`; at most 28 characters |
| `name` | string | Required | At most 1000 characters; this is the name shown in the list of SPAs |
| `url` | string | Required | At most 1000 characters; the part of the SPA's address you choose |
| `clientscriptfile` | filereference | Required | At most 199 characters, extension included |
| `serverscriptfile` | filereference | Required | At most 199 characters, extension included |
| `folder` | folderreference | Required | The SPA's own folder |
| `description` | string | Optional | At most 1000 characters |
| `assetsfolder` | folderreference | Optional | Folder for stylesheets, images and other static files |
| `audienceallroles` | boolean | Optional | Defaults to `F` |
| `audienceroles` | multi-select | Optional | Role references separated by `\|` |
| `executeas` | single-select | Optional | Default given as `CURRENT ROLE` [sic] (see below) |
| `loglevel` | single-select | Optional | Defaults to `DEBUG` |
| `usesuif` | boolean | Optional | Defaults to `T` |
| `notifyadmins` | boolean | Optional | Defaults to `F` |
| `notifyowner` | boolean | Optional | Defaults to `T` |
| `notifyuser` | boolean | Optional | Defaults to `F` |
| `notifyemails` | string | Optional | Valid email addresses separated by commas; at most 999 characters |
| `links` | structure | Optional | Contains `link` elements (see [Links](#links)) |

## Choosing the URL

The `url` value becomes the custom segment of the app's address. Keep it lower case, put no spaces
in it and do not reuse a value another SPA already has. Two sets of characters are off limits:

- the URL reserved symbols `!` `*` `'` `(` `)` `;` `:` `@` `&` `=` `+` `$` `,` `/` `?` `#` `[` `]`;
- the characters `` ` `` `{` `}` `^` `"` `|` `<` `>` `%`, which make the request fail with a bad
  request error.

## Script files and folders

- `clientscriptfile` points at the code that runs the app in the user's browser.
  `serverscriptfile` points at code that runs on the server before the app is handed to the
  browser. Both must be `.js` files and both must live inside the SPA folder.
- Paths in `clientscriptfile`, `serverscriptfile`, `folder` and `assetsfolder` are written relative
  to the project's `src/FileCabinet` folder.
- The SPA folder's location depends on the project type: under
  `src/FileCabinet/SuiteApps/<ApplicationId>/` in a SuiteApp project, under
  `src/FileCabinet/SuiteScripts/` in an account customization project.
- Each SPA needs a folder of its own; two SPAs cannot point at the same one.
- Names of the SPA folder and the assets folder are limited to 99 characters and must not contain
  `?` `*` `<` `>` `|` `'` `\` `!` `:` `/`.
- The assets folder belongs inside the SPA folder; that is where subfolders for styles, images and
  the like go.

### Serving assets

A file in the assets folder is reachable at an endpoint whose shape depends on the project type.
`<file>` is the file's path relative to the assets folder.

| Project type | Asset endpoint |
|---|---|
| SuiteApp | `/spa-app/<appid>/<spa_folder>/assets/<file>` |
| Account customization project | `/spa/<url_value>/assets/<file>` |

In the account customization form, `<url_value>` is the value of the object's `url` element.

## Audience and execution role

- With `audienceallroles` set to `T`, every internal role can open the SPA, plus whichever external
  roles `audienceroles` lists. The default is `F`.
- If `audienceroles` has values and you drop `audienceallroles`, the audience narrows to just the
  roles in `audienceroles`.
- `audienceroles` and `executeas` both take role references. The built-in role IDs are in the
  [generic_role](../gen/catalogs/sdf_list_generic_role.md) value list (see
  [SDF value lists](sdf.value_lists.md)). `audienceroles` holds several roles joined by `|`; for
  example a role defined in another SuiteApp followed by a built-in role:
  `[appid=<appid>, scriptid=<role_scriptid>]|DEVELOPER`.
- `executeas` decides which role the **server script** runs under. `ADMINISTRATOR` is not an
  allowed value. Code running in the browser always runs with the role of the logged-in user,
  whatever `executeas` says.

> [!SOURCE-ERROR] The source states the `executeas` default as `CURRENT ROLE` [sic], spelled with a
> space, and no such value exists in the generic_role list that the field takes its values from.
> Don't write that literal into your XML; omit `executeas` to get the default, or set a specific
> role and validate the project.
<!-- source-error: topic:sdf.singlepageapp | - | executeas_default_current_role_not_in_generic_role_list -->

## Logging, UIF and notifications

- `loglevel` takes a value from the
  [script_loglevel](../gen/catalogs/sdf_list_script_loglevel.md) list and is `DEBUG` unless you set
  it.
- `usesuif` is `T` by default, and it has to be `T` whenever the app uses the UIF libraries.
- Notification defaults: `notifyowner` is `T`, `notifyadmins` and `notifyuser` are `F`.
  `notifyemails` takes extra recipients as a comma-separated list of valid addresses, 999
  characters at most in total.

## Links

`links` is the structure that places links to the SPA in the NetSuite UI. It contains `link`
elements, and each `link` has two fields:

| Element (under `links.link`) | Type | Use | Meaning |
|---|---|---|---|
| `linkcategory` | single-select list | Required in every `link` | Where the link shows up: a center category |
| `linklabel` | single-select list | Required in every `link` | The text shown for the link in the UI; at most 100 characters |

- As soon as a `link` element is present, both of its fields must be filled in.
- `linkcategory` accepts either a built-in center category from the
  [generic_centercategory](../gen/catalogs/sdf_list_generic_centercategory.md) value list (upper-case
  IDs such as `BASICSUPPORTCUSTOMERSERVICE` or `ACCOUNTCENTERFINANCIALBANKING`) or a reference to a
  `centercategory` object.
- `linklabel` can also be a reference to a translatable `string`, so the label can come from a
  [translation collection](sdf.translation_collections.md) instead of fixed text. The source does
  not show the reference syntax for such a term.

## Example

A field-service dispatch board deployed from an account customization project. All IDs, folder
names and the URL are invented, and the `url` keeps to lower-case letters and a hyphen. The first
link uses a built-in center category; the second points at a `centercategory` object assumed to
exist in the same project. The file is saved as `custspa_dispatch_board.xml`.

```xml
<singlepageapp scriptid="custspa_dispatch_board">
  <name>Dispatch Board</name>
  <url>dispatch-board</url>
  <description>Assigns open service calls to technicians.</description>
  <folder>[/SuiteScripts/DispatchBoard/]</folder>
  <clientscriptfile>[/SuiteScripts/DispatchBoard/BoardClient.js]</clientscriptfile>
  <serverscriptfile>[/SuiteScripts/DispatchBoard/BoardServer.js]</serverscriptfile>
  <assetsfolder>[/SuiteScripts/DispatchBoard/assets/]</assetsfolder>
  <audienceallroles>F</audienceallroles>
  <audienceroles>SUPPORT_MANAGER|SUPPORT_PERSON</audienceroles>
  <executeas>SUPPORT_MANAGER</executeas>
  <loglevel>AUDIT</loglevel>
  <usesuif>T</usesuif>
  <notifyowner>T</notifyowner>
  <links>
    <link>
      <linkcategory>BASICSUPPORTCUSTOMERSERVICE</linkcategory>
      <linklabel>Dispatch Board</linklabel>
    </link>
    <link>
      <linkcategory>[scriptid=custcentercategory_field_service]</linkcategory>
      <linklabel>Open dispatch board</linklabel>
    </link>
  </links>
</singlepageapp>
```

With this object, a stylesheet stored at `assets/styles/board.css` inside the SPA folder is served
at `/spa/dispatch-board/assets/styles/board.css`.

The source does not state the exact syntax of folder references in `folder` and `assetsfolder`;
the bracketed File Cabinet paths above follow the general reference style described in
[SDF custom object files](sdf.object_files.md#referencing-other-objects-and-files). Confirm them
with a validation run.
