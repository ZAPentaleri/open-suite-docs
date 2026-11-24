# Single Page Application Management
<!-- id: topic:single_page_applications.management -->

Part of: [Single Page Applications](single_page_applications.md)

See also: [SDF singlepageapp object](sdf.singlepageapp.md),
[Single Page Application Development](single_page_applications.development.md).

Once a single page application (SPA) is deployed, you look after it from a management page in the
NetSuite UI. This page covers what can be changed there, the default values, and how redeploying the SPA
object XML interacts with those settings.

## Where to find it and who can use it

- The SPA list is in the Customization menu, under Scripting, as Single Page Applications.
- You need the SuiteScript permission at the Edit or Full level.
- Clicking an SPA's name in the list opens its management page. The link icon next to the name opens the
  SPA itself; the link icon next to its source opens that location in the File Cabinet.

## Tabs and settings

The management page covers these settings: details, roles, log level, audience, links, error
notifications, logs, deletion and system notes. They are spread over four tabs: **Basic Info**,
**Configuration**, **Logs** and **System Notes**.

**Basic Info** shows the SPA name, the SPA ID, the URL, the sources, and the name and ID of the SuiteApp
the SPA belongs to.

For an SPA that was installed with a Marketplace SuiteApp, the Basic Info tab is read-only while the
Configuration tab can still be edited. SuiteApp installation preferences, including locking, can also be
applied to an SPA; the source does not spell out which settings that locking covers.

The fields of the SPA object XML correspond to the fields on the SPA record's Basic Info and
Configuration tabs.

### Execute as

Defaults to the current role. Whatever you pick, the part of the SPA that runs in the browser always runs
as the user who is logged in.

### Log level

The options are Debug, Audit, Error and Emergency, with Debug as the default. The chosen level filters
which entries appear on the Logs tab. The Logs tab has a refresh control that loads any new entries.

### Release audience

You can pick internal roles, external roles, or all internal roles. With an empty audience, only the
script owner can run the SPA.

### Center links

A center link gives users a menu path to the SPA.

- Each menu location can hold only one center link.
- If the insert-before setting is empty, the new link goes to the bottom.
- Reload the page after adding a link to see it.

### Error notifications

By default the script owner is notified. The choices are the current user, the script owner, all
administrators, groups (picked from the groups list), and specific email addresses.

### System notes

The System Notes tab lists the date and time, user, role, action type, object, old and new values, and
deletions. Changes to the audience, center links and error notifications are not recorded there.

## Updating an SPA through its object XML

The UI offers no button to download the XML of an SPA object, so a deployed SPA is changed by redeploying
its SDF object:

1. Import the SPA object into a project with the SuiteCloud CLI or the IDE.
2. Download the SPA's files, or create them locally.
3. Edit the XML definition (its elements are described on [SDF singlepageapp object](sdf.singlepageapp.md)).
4. Deploy the project.

Redeploying requires the local project files. An empty assets folder is not downloaded on import.

What a redeploy does:

- If the SPA ID changes, NetSuite treats the SPA as a new one. When you change the ID, update the URL and
  the folder too.
- Updating the XML does not create the files it points to; they must already exist.
- Removing an optional element from the XML keeps the current value.
- Leaving an optional element present but empty resets that setting to its default; for `notifyowner`, an
  empty value becomes false.
- `audienceallroles` selects internal roles only; add external roles through `audienceroles`.
