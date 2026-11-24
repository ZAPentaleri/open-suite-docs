# Single Page Application Development
<!-- id: topic:single_page_applications.development -->

Part of: [Single Page Applications](single_page_applications.md)

See also: [SDF singlepageapp object](sdf.singlepageapp.md), [N/runtime](../modules/N_runtime.md),
[N/record](../modules/N_record.md).

This page covers how an SPA project is set up, the two SPA scripts and their entry points, the SPA
object XML and its validation rules, and the errors you can hit at deploy time and at run time.

## Prerequisites

- An SDF SuiteCloud project. SPA scripts are SuiteScript 2.1.
- The project manifest must declare the `SERVERSIDESCRIPTING` feature as a dependency.
- Each SPA has exactly one folder and exactly one object XML file.

You can start from one of the sample SPA SuiteApps. If you do, rename its folders, its files and
the names used inside those files, so your SPA does not clash with the sample.

The WebStorm SuiteCloud plugin offers neither an SPA object type nor SPA script templates. In
WebStorm, copy a sample SPA project (or the structure of an SPA you already have) instead.

## Building an SPA, step by step

1. Create an SDF project with the SuiteCloud CLI for Node.js.
2. Set up the target account with the same CLI.
3. Add the `SERVERSIDESCRIPTING` feature dependency to the manifest.
4. Create the SPA folder, the client and server scripts, any assets, and the SPA object XML.
5. Create the root UIF component.
6. Build and bundle (the samples use `npm run build` and `npm run bundle`).
7. Validate the project and deploy it with the CLI, or run `npm run deploy` in the samples, which
   runs the bundle and then the SuiteCloud `project:deploy` command.
8. Open the SPA.

The source names these CLI steps (create a project, set up the account, validate, deploy) but does
not spell out the command names for creating, setting up and validating; check the CLI's own help
for them.

In short: create the project, add the SPA folder, scripts and object, build the UI with the UIF,
transpile and build, then validate and deploy.

The sample scripts use `import`, so they must be transpiled before NetSuite can run them. The build
writes the transpiled files to a File Cabinet path in the project that SDF can deploy. You can also
install the UIF TypeScript type definitions as a development dependency to get typing in your
editor.

## Project structure

- The SPA folder holds the client script, the server script and, optionally, an assets folder.
- The client and server script paths in the object XML must point inside the SPA folder.
- If the object sets an assets folder, that folder must be inside the SPA folder.
- The object's `scriptid` must be the same as the XML file name.
- Object paths point into `src/FileCabinet/<SuiteCloudProjectRoot>/`. The project root is
  `SuiteApps/<ApplicationId>` in a SuiteApp project and `SuiteScripts` in an account customization
  project.
- Paths are written from `src/FileCabinet` as the root, starting with `/`. Script paths must lie
  under the project root folder (`/<SuiteCloudProjectRoot>/`) or under `/Web Site Hosting Files/`.

> [!SOURCE-ERROR] The source describes the SPA object's paths as relative to `src/FileCabinet` in
> one place and as absolute in another. Write them starting with `/` from `src/FileCabinet`, as above,
> and confirm with a validation run that the deploy accepts them.
<!-- source-error: topic:single_page_applications.development | - | spa_object_paths_called_relative_and_absolute -->

- Custom stylesheets typically go into an `assets/css` folder.

### Asset URLs

Files in the assets folder are served from these addresses (under the account's app domain):

- SPA in a SuiteApp: `spa-app`, then the application ID, the SPA folder, `assets` and the file's
  path.
- SPA in an account customization project: `spa`, then the custom URL, `assets` and the file's
  path.

The literal path patterns are on [Single Page Applications](single_page_applications.md#urls). The
spec's example is a logo image in the assets folder of a hello-world SPA. If no assets folder is
defined, asset URLs return 404.

## The SPA server script

The server script is the SPA's entry point on the server. It works as a plugin that can change the
response NetSuite sends for the SPA, and it can load most server-side SuiteScript modules.

- **How it is declared.** The script type is set entirely by the JSDoc block: the file must carry
  `@NApiVersion 2.1` and `@NScriptType SpaServerScript`. This reference has no separate script-type
  page for it; its entry point and context are described here.
- Entry point: `initializeSpa(context)`.
- Context member: `context.addStyleSheet`, which adds a stylesheet to the response. Its parameters
  are `relativePath` and `url`. One of the two is required; if both are given, `url` takes
  precedence. The spec's example is a relative path to a `main.css` file. The source does not state
  how the parameters are passed or what `relativePath` is relative to.
- If the server script throws, NetSuite shows an error message and does not serve the SPA. When the
  thrown value is a string, that string appears in the error message.

The spec's samples for the server script check the current user's email address (with
[N/runtime](../modules/N_runtime.md)) and add a custom stylesheet.

## The SPA client script

The client script loads the UIF components in the browser. The source does not state any JSDoc
tags for it.

- Entry point: `run(context)`. It may return any value, including a promise; the loading icon stays
  until `run` returns or the promise settles.
- Context members:

| Member | What it does |
|---|---|
| `context.baseUrl` | The SPA's base URL |
| `context.setLayout` | Chooses the layout: `application` fills the window height; `natural` is only as tall as the content needs |
| `context.setContent` | Attaches the root component to the UI |

- The root component must be in its own UIF file and must be passed to `setContent`. The spec's
  example is a hello-world root component.
- The client script can use [N/record](../modules/N_record.md); the spec's sample loads an employee
  record from the SPA client. Other examples it mentions are sending an email with the current user
  as author and setting notes on an employee record.

## The SPA object XML

Object script IDs must start with `custspa`. The full element reference is on
[SDF singlepageapp object](sdf.singlepageapp.md); the validation rules are:

| Element | Rule |
|---|---|
| `scriptid` | Required. Lowercase letters, digits and underscores only; at most 28 characters |
| `name` | Required. At most 1,000 characters |
| `url` | Required. At most 1,000 characters; unique in the account; no spaces or reserved punctuation (see below) |
| `folder` | Required. Starts and ends with `/`; folder name at most 99 characters; unique in the account |
| `clientscriptfile` | Required. A `.js` file; file name at most 199 characters; path starts with `/<SuiteCloudProjectRoot>/` or `/Web Site Hosting Files/` |
| `serverscriptfile` | Required. Same rules as `clientscriptfile` |
| `description` | At most 1,000 characters |
| `assetsfolder` | Starts and ends with `/`; must be inside the SPA folder |
| `linkcategory`, `linklabel` | Both required when a link element is present; `linklabel` at most 30 characters; only one link per link category |
| `notifyemails` | Valid email addresses separated by commas; at most 999 characters |
| `executeas` | An uppercase generic role value; the administrator role is not allowed |
| `audienceroles` | Uppercase generic role values; custom roles must be defined in the project or its dependencies |
| `loglevel` | One of `DEBUG`, `AUDIT`, `ERROR`, `EMERGENCY` |
| `audienceallroles`, `notifyuser`, `notifyowner`, `notifyadmins` | `T` or `F` |

Characters that are not allowed:

- in `url`: a space and `?` `!` `*` `'` `(` `)` `;` `:` `@` `&` `=` `+` `$` `,` `/` `#` `[` `]`;
- in `folder` and `assetsfolder`: `?` `*` `<` `>` `|` `'` `\`.

`linkcategory` takes a value from
[catalog:sdf_list_generic_centercategory](../gen/catalogs/sdf_list_generic_centercategory.md), or a
reference to a custom center category object; such a referenced object must be part of the project.

The folder, publisher and project IDs used in the object must match those in the manifest and in
`deploy.xml`.

## Errors

Errors come in two kinds: those raised while validating and deploying the project, and those raised
when the SPA runs.

### Validation and deployment errors

They fall into these groups:

- missing fields,
- incorrect format,
- duplicate values,
- invalid values,
- values that do not match (for example IDs that differ between object, manifest and deploy file),
- a referenced file or folder that does not exist.

### Execution errors

Typical causes: the SPA does not exist, a file is inactive, a file permission blocks access, the
user's role has no access to the SPA, a dependency is missing, or the `run` function is missing.

| Error code | When it happens |
|---|---|
| `SCRIPT_FILE_DEPENDENCY_ERROR` | The client script loads a module that cannot be found |
| `SCRIPT_DOES_NOT_HAVE_RUN_MEMBER` | The client script has no `run` function |
| `SCRIPT_MEMBER_NOT_CALLABLE` | `run` exists but is not a function |
| `SCRIPT_DOES_NOT_EXPOSE_ANY_MODULE` | The script has no `define` call or returns nothing |
| `SCRIPT_ENTRY_POINT_THROWS_ERROR` | `run` throws |

If the SPA folder is private, users other than its owner get a rendering error.

## Opening and finding an SPA

Deployed SPAs are listed under Customization > Scripting > Single Page Applications. The link in
the URL field of an SPA's record opens the application.

## Example: server script

SPA sources are normally written as ES modules and converted by the build. The block below shows
the AMD form that the File Cabinet holds after the build. It follows the spec's sample idea of
checking the current user's email. Passing `addStyleSheet` its parameter in one object, and the form
of the path, are assumptions here, since the source does not state them. The domain check itself is
plain JavaScript.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType SpaServerScript
 */
define(['N/runtime'], (runtime) => {
  const ALLOWED_DOMAIN = '@harbor-logistics.example';

  const initializeSpa = (context) => {
    const email = runtime.getCurrentUser().email || '';
    if (!email.toLowerCase().endsWith(ALLOWED_DOMAIN)) {
      throw `Dock scheduler is limited to ${ALLOWED_DOMAIN} staff.`;
    }
    context.addStyleSheet({ relativePath: 'assets/css/dock-board.css' });
  };

  return { initializeSpa };
});
```
