# SuiteScript environment setup
<!-- id: topic:dev_environment -->

Part of: [SuiteScript Developer Guide](developer_guide.md)

See also: [SuiteScript IDs](suitescript_ids.md), [Records Browser](records_browser.md),
[SuiteScript Debugger](debugger.md).

Getting an account ready for SuiteScript work takes four steps:

1. Enable client and/or server SuiteScript.
2. Set up roles and permissions.
3. Turn on the display of internal IDs.
4. Set up your editor or IDE.

Plan for the version deadline too: scripts written in SuiteScript 1.x or 2.x (2.0) should be updated
to 2.1 by release 2028.2.

## Enabling SuiteScript

An administrator enables the features at **Setup > Company > Enable Features**, on the **SuiteCloud**
subtab. Once client SuiteScript is on, forms get a **Custom Code** subtab. Enabling SuiteScript also
creates a SuiteScripts folder in the File Cabinet.

## Roles and permissions

Permissions are managed at **Setup > Users/Roles > Manage Roles**, on the role's **Permissions >
Setup** subtab. The **SuiteScript** permission has four levels:

| Level | Lets the user |
|---|---|
| View | View script and script deployment records |
| Create | Create those records, and download the SuiteScript library file |
| Edit | Edit script and deployment records |
| Full | Save and execute scripts, and debug 2.1 scripts |

Other permissions you may need:

- **SuiteScript Scheduling** at Full level, to submit scheduled or map/reduce scripts through the API
  and to read a script's percent-complete status.
- **SuiteScript** at Full level to debug 2.1 scripts.
- Form customization permissions, when a custom role is to work with SuiteScript.

The Administrator role is the recommended choice when someone needs full SuiteScript access.

To run scripts for users in the Customer, Vendor, Partner or Employee Center, add the center role on
the deployment's **Audience** subtab (see [runtime options](runtime_options.md)).

## Where script files live

- Only scripts stored in the File Cabinet run, and they can sit in any folder.
- Script files must allow company-wide usage, or be available without login.
- Outside the SuiteCloud IDE tools you upload files by hand. Uploading a file with the same name
  again asks whether to overwrite it.

## Editors and the library file

You can use the SuiteCloud extension for Visual Studio Code, the SuiteCloud plug-in for WebStorm, or
any editor you like. For code completion, download the SuiteScript 2.1 API library file from the File
Cabinet (there is a link for it there); downloading it requires the SuiteScript permission.

## Internal IDs

Scripts refer to records and fields by internal ID, always in lower case.

- Turn IDs on under **Set Preferences > General > Show Internal IDs**. The same preference shows field
  IDs.
- An internal ID does not change when the record's values change.
- A custom record type can hold up to 2,000,000,000 instances. When that internal ID limit is reached,
  creating another record fails with an error; existing records are not affected.

### Finding a record's ID

- The **Internal ID** column of a list, for example **Lists > Relationships > Customers**.
- The record's link: the ID is a URL parameter. Hover over the link to see it in the browser's status
  bar; a customer such as "ABC Co." might show ID 1766.
- The [Records Browser](records_browser.md).

### Finding a field's ID

- Click a field label to open its help pop-up, which shows the field ID (the Company field, for
  instance, is `companyname`).
- The **ID** column of the custom field lists, for example **Customization > Lists, Records, &
  Fields > CRM Fields**.
- The [Records Browser](records_browser.md). Some fields are hidden from users and can only be found
  there.

Give your own custom fields deliberate IDs rather than accepting generated ones.
