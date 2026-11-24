# Scriptable template synthetic fields: records
<!-- id: topic:scriptable_templates.synthetic_fields_records -->

Part of: [Scriptable email templates](scriptable_templates.md)

See also: [Scriptable template synthetic fields: notifications](scriptable_templates.synthetic_fields_notifications.md).

Synthetic fields are extra values a scriptable template can print even though SuiteScript has no
access to them. This page covers the ones tied to records, to the sending user, to preferences and to
company information. For the user, customer, subsidiary, sales rep and case groups the source names
the fields but not the object you reach them through; only the preference, company information and
subscription center fields are given with their prefix.

## Case

| Field | Meaning |
|---|---|
| `issues` | Sublist of the issues related to the case |

## Sending user

| Field | Meaning |
|---|---|
| `isSystem` | True when the system generated the email; false when a user initiated it |
| `firstName`, `middleName`, `lastName` | The sender's name parts |
| `phone` | The sender's phone number |
| `email`, `altEmail` | The sender's email address and alternate email address |

The fields from `firstName` through `altEmail` are only filled for email a user initiated, and their
values are taken from the sending employee's record.

## Preferences

Each `preferences.*` field returns the value of a preference. The label of the preference in the UI is
given verbatim below.

| Field | Preference label |
|---|---|
| `preferences.message_signature` | "Signature" |
| `preferences.message_nickname` | "Nickname" |
| `preferences.timezone` | "Time Zone" |
| `preferences.naming_customer` | "Name for Customer" |
| `preferences.naming_lead` | "Name for Lead" |
| `preferences.issue_multiple_versions` | "Use Multiple Versions and Builds" |

Where these preferences are set:

- personal preferences: `Home > Set Preferences`;
- the naming preferences: the `Rename Records/Transactions` page;
- the issue preference: `Setup > Issues > Issue Preferences`.

When "Use Multiple Versions and Builds" is on, versions and builds come through as sublists of values
rather than single values. Either way, a template can still show the build and version with a
directive; the source does not name the directive.

## Company information

| Field | Setting label | What it holds |
|---|---|---|
| `companyInformation.logoUrl` | "Company Logo (Forms)" | The company logo set for forms |
| `companyInformation.adminEmail` | "Administrator Email Address" | The address used for login access notifications |
| `companyInformation.loginURL` | "Login URL" | The URL included in the login access notification email |

The company logo setting is found under `Setup > Company > Company Information`.

## Customer

| Field | Meaning |
|---|---|
| `url` | URL of the customer record |
| `relatedItems` | Sublist of related items |
| `correlatedItems` | Sublist of correlated items |

Fields of each line:

| Field | In `relatedItems` | In `correlatedItems` | Meaning |
|---|---|---|---|
| `purchasedItem` | Yes | Yes | Name of the item that was bought |
| `upsellItem` | Yes | Yes | Name of the related or upsell item |
| `sellable` | Yes | Yes | Whether the item is available for sale |
| `correlation` | No | Yes | How strongly the upsell item correlates with the purchased item |
| `count` | No | Yes | How many times the same customer bought both items |

## Subsidiary

| Field | Meaning |
|---|---|
| `logourl` | The logo set for the subsidiary's forms |
| `pagelogourl` | The logo set for the subsidiary's pages |

## Sales rep

These are only filled when the customer has a sales rep assigned.

| Field | Meaning |
|---|---|
| `firstName`, `middleName`, `lastName` | The sales rep's first, middle and last name |
| `phone` | The sales rep's phone number |
| `email` | The sales rep's email address |
| `altEmail` | The sales rep's alternate email address |
| `MESSAGE_NICKNAME` | Value of the sales rep's "Nickname" preference |
| `MESSAGE_SIGNATURE` | Value of the sales rep's "Signature" preference |

The last two are written in capitals, as the source lists them. The sales rep sets those two
preferences under `Home > Set Preferences > General`.

## Subscriptions

These need the `Subscription Categories` feature.

| Field | Meaning |
|---|---|
| `subscriptions` | Sublist of subscription categories |
| `subscription` | Name of the category |
| `subscribed` | The recipient's status for that campaign category |

The `subscriptions` sublist is available on customer, vendor, contact, employee and partner records.

The subscription center adds two links:

| Field | Meaning |
|---|---|
| `subscriptionCenter.unsubscribeURL` | Opens the subscription center with every category unchecked |
| `subscriptionCenter.manageURL` | Opens the subscription center with categories checked according to the recipient's current settings |

## Example

A newsletter footer written for this page, using only fields whose prefix the source gives: the
company logo, the sender's signature, the sender's time zone, and the two subscription center links.

```ftl
<table>
  <tr>
    <td><img src="${companyInformation.logoUrl}" width="160" height="48"/></td>
    <td>${preferences.message_signature}<br/>Office hours are given in ${preferences.timezone}.</td>
  </tr>
</table>
<p>
  <a href="${subscriptionCenter.manageURL}">Choose which updates you receive</a> |
  <a href="${subscriptionCenter.unsubscribeURL}">Stop all marketing email</a>
</p>
```
