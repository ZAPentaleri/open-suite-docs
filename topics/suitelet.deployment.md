# Suitelet Deployment Record
<!-- id: topic:suitelet.deployment -->

About: [Suitelet](../script-types/Suitelet.md), [script deployment record (`scriptdeployment`)](../gen/records/scriptdeployment.md).

See also: [Script Deployment](script_deployment.md), [Suitelet Samples](suitelet.samples.md),
[SDF Suitelet deployments](../gen/catalogs/sdf_suitelet_deployments.md).

A [Suitelet](../script-types/Suitelet.md) is reachable only through a deployment. Each Suitelet script
record needs at least one deployment record, and that deployment controls who may open the Suitelet,
whether a login is needed, and where menu links to it show up in NetSuite centers.

## Creating and opening a deployment

- When you view a script record, its **Deploy Script** button opens a new deployment for that script.
- To change an existing Suitelet deployment, edit it from the list of script deployments.
- Some body fields are shown only when you view or edit a deployment that already exists.
- Besides the fields that every deployment has, a Suitelet deployment adds the **Available Without
  Login** box, an **Audience** subtab and a **Links** subtab.

## Body fields

| Field | What it does |
|---|---|
| Script | Links the deployment to its script record. The link cannot be changed afterwards; if you picked the wrong script, start over with a new deployment. |
| ID | Lower case, no spaces, underscores between words; NetSuite puts `customdeploy` in front of it. Supply your own ID when the deployment will be bundled. |
| Deployed | The Suitelet runs only while this box is checked. |
| Status | **Testing**: only the owner and the audience you chose can run it. **Released**: everyone in the specified audience can run it. |
| Event Type | Restricts the script to the selected event and overrides the event types handled in the script file. Left blank, the script runs for the types the file handles. |
| Log Level | `Debug`, `Audit`, `Error` or `Emergency`. |
| Available Without Login | Lets users with no active NetSuite session open the Suitelet (see below). |

The source illustrates Event Type with Delete: selecting it makes the script run only on delete, even
when the script file also handles create.

> [!SOURCE-ERROR] This Event Type example is built on record events such as create and delete, which a
> Suitelet never receives, so the field and its example appear to be carried over from record-based script
> types. Do not depend on Event Type to filter Suitelet requests; test its effect in your account.
<!-- source-error: topic:suitelet.deployment | - | event_type_example_uses_record_events_not_in_suitelets -->

## Audience subtab

Six fields make up the audience: internal roles, external roles, groups, partners, departments and
employees.

- At least one internal or external role is required.
- An empty groups or departments field means all groups or all departments.
- There are select-all choices for internal roles, for partners (all partners) and for employees (all
  employees).
- Give access only to the roles that really need the Suitelet.

## Links subtab

The Links subtab places menu links to the Suitelet in NetSuite centers. Each link has a center, a
section, a category, a label, a translation and an insert-before position. The label is the text that
appears in the chosen section's menu, after the category. Translations need the feature `MULTILANGUAGE`
(shown as Multi-Language in [catalog:features](../gen/catalogs/features.md)). One example use is a link
in the Support section of the Classic center.

For a user to reach the Suitelet through such a center link, their role has to be on the Audience subtab,
and in addition either the script's files must be available company-wide or the deployment must be
available without login.

## Available Without Login

Checking **Available Without Login** on the script deployment opens the Suitelet to people who have no
NetSuite session. After the deployment is saved, it displays an external URL for the Suitelet.

Typical uses:

- one-off online forms, such as registration for a partner conference;
- inbound notifications from partners, such as payment notifications from PayPal;
- an unsubscribe page for an email campaign;
- a third-party web site that posts data into NetSuite.

Keep in mind:

- Only some APIs work without a session. The full API needs a valid NetSuite session, which means the
  user has logged in with email and password.
- Data handled by a Suitelet that does not require authentication is less secure.
- Suitelets are not meant for system-to-system integration.

### Who can run it

Success depends on three things: which URL is used (internal or external), the deployment status, and,
for the external URL, whether the Online Form User role is one of the external roles.

| URL | Status | Caller | Result |
|---|---|---|---|
| Internal | Testing | Any user | Runs |
| Internal | Released | User in the audience | Runs |
| Internal | Released | User outside the audience | Error: the user lacks privileges |
| External | Testing | Anyone | Error: direct navigation is not allowed |
| External | Released | Online Form User role among the external roles | Runs |
| External | Released | Online Form User role not selected | Error: the user lacks privileges |

Setting it up:

- **Internal access**: select all internal roles.
- **External access**: put the Online Form User role in the external roles and leave the other audience
  fields empty.
- The Online Form User role is available only when the feature `"Online Forms"` is enabled (feature ID
  `EXTCRM`; see [catalog:features](../gen/catalogs/features.md)). You turn it on in Enable Features, on the
  CRM subtab, in the Marketing section.
- If the role is still missing after you enable the feature, log out and log back in.

## SDF

When deployments are maintained with SDF, their XML fields are listed in the
[SDF Suitelet deployments](../gen/catalogs/sdf_suitelet_deployments.md) catalog.
