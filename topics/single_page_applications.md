# Single Page Applications
<!-- id: topic:single_page_applications -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

Sub-topics:

- [Single Page Application Development](single_page_applications.development.md)
- [Single Page Application Management](single_page_applications.management.md)

See also: [SDF singlepageapp object](sdf.singlepageapp.md) (the SDF object that defines an SPA).

A Single Page Application (SPA) is a custom user interface that runs inside NetSuite. The
application code is sent to the browser once; after that, only the parts of the page that change
are updated, with no full reload. The HTML is rendered in the browser, not on the server. For new
custom UI work, SPAs are the recommended alternative to Suitelets.

## What an SPA is built from

An SPA is built with, and requires, three technologies:

- the **NetSuite UIF** (User Interface Framework), which provides components, routing, state
  management and internationalization;
- **SuiteScript**, for the server-side and client-side scripts;
- the **SuiteCloud Development Framework (SDF)**, which packages and deploys the application.

Every SPA needs four pieces:

1. its own SPA folder (each application gets a folder of its own),
2. an SPA client script,
3. an SPA server script,
4. an SPA object definition, the SDF object XML described in
   [SDF singlepageapp object](sdf.singlepageapp.md).

The build recognizes which folder is an SPA folder by the SPA server script inside it. Oracle
publishes sample SPA SuiteApps that you can copy and use as a starting template.

What you gain over a classic page:

- no page reloads while the user works,
- less bandwidth used,
- presentation kept apart from business logic,
- a result that looks like the rest of NetSuite.

## Request lifecycle

When someone opens an SPA, these steps run in order:

1. NetSuite checks that the user is in the SPA's audience.
2. The server script's `initializeSpa` entry point runs.
3. The page loads.
4. The client script's `run` entry point runs.
5. The content is shown.

The server script is where you validate the request and adjust the response; through its context
it can, for example, add stylesheets to the response.

In the browser, the client script loads only after the NetSuite center elements are in place. A
loading icon stays on screen until `run` returns or, if `run` returns a promise, until that promise
settles.

An SPA is subject to a time limit of 300 seconds; see
[catalog:other_time_limits](../gen/catalogs/other_time_limits.md).

The entry points and their context members are described in
[Single Page Application Development](single_page_applications.development.md).

## URLs

The address of an SPA, under the account's app domain, depends on how it was delivered:

| Delivered in | SPA path | Asset path |
|---|---|---|
| A SuiteApp | `/spa-app/<ApplicationId>/<CustomURL>` | `/spa-app/<ApplicationId>/<SpaFolder>/assets/<path>` |
| An account customization project (ACP) | `/spa/<CustomURL>` | `/spa/<CustomURL>/assets/<path>` |

`<CustomURL>` is the URL set in the SPA object, `<ApplicationId>` the SuiteApp's application ID,
`<SpaFolder>` the SPA folder and `<path>` the file's path inside the assets folder.

## UIF tooling

- Type definitions for the UIF come as a package that the spec names `@netsuite-uif-types`.

> [!SOURCE-ERROR] The sources disagree on the name of the UIF type-definition package: the page on
> creating an SPA names an Oracle SuiteCloud UIF types package instead. The spec keeps the name shown
> above; check the actual package name in the npm registry before you install it.
<!-- source-error: topic:single_page_applications | - | creation_page_names_package_oracle_suitecloud_uif_types -->

- The **UIF catalog** is opened from the account's app domain, in the UI apps catalog area. It
  lists the available components together with samples and a sandbox for trying them out.

## Module systems and the build

UIF code and SuiteScript use different module systems:

| Side | Module system |
|---|---|
| UIF | ES modules (`import` / `export`) |
| SuiteScript | AMD, loaded by RequireJS (`define` / `require`) |

SPA scripts are written with JSX and ES modules, in JavaScript or TypeScript. Every JavaScript file
that ends up in the File Cabinet must be AMD, so a build step is needed: it transpiles the JSX and
rewrites `import` / `export` into `define` / `require`. The samples drive this build with a
`gulpfile.mjs`.

Build rules:

- Only sources inside the SPA source folder are built; anything outside it is ignored.
- The bundle's entry points are `SpaClient`, `SpaServer`, and any script tagged with
  `@NScriptType`.
- Files in the SPA source folder that are not JavaScript or TypeScript are treated as assets.
- Two build options shape the output: `concatenateScripts` and `minifyScripts`.
- Ordinary AMD SuiteScript files must stay outside the SPA output folder in the File Cabinet. AMD
  scripts placed inside the SPA folder cause conversion errors.
- Each bundle run regenerates the SPA output folder, so anything you put there by hand is lost on
  the next run.

Where to keep ordinary AMD scripts in the project:

- SuiteApp: `src/FileCabinet/SuiteApps/<ApplicationId>/`
- Account customization project: `src/FileCabinet/SuiteScripts/`

### Sample project scripts

The sample projects define these npm scripts:

| Script | What it does |
|---|---|
| `build` | Runs the gulp build, compiling TypeScript into the `/build/` folder |
| `bundle` | Runs the gulp bundle, which turns the entry points into AMD files and copies the assets into the SPA output folder in the File Cabinet |
| `clean` | Runs the gulp clean, removing the build folder and the SPA output folder in the File Cabinet |
| `deploy` | Runs `bundle`, then deploys with the SuiteCloud `project:deploy` command |
| `test` | Runs the Jest unit tests kept in the `test/unit` folder |

### Project layout

A sample SPA project contains:

- the SPA source folder,
- `/test` for tests,
- `/src/manifest.xml` and `/src/deploy.xml` (the SDF project files),
- `/src/Objects/custspa_*.xml`, the SPA object definitions (see
  [SDF singlepageapp object](sdf.singlepageapp.md)),
- the SPA output folder in the File Cabinet part of the project.

No code example is given on this page: SPA sources use JSX and ES modules, which become runnable
SuiteScript only after the build. See
[Single Page Application Development](single_page_applications.development.md) for the entry
points and an example of the built server script.
