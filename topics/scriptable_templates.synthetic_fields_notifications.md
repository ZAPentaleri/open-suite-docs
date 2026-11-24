# Scriptable template synthetic fields: notifications
<!-- id: topic:scriptable_templates.synthetic_fields_notifications -->

Part of: [Scriptable email templates](scriptable_templates.md)

Synthetic fields are values NetSuite computes for a template at send time. They exist only inside
scriptable templates; SuiteScript cannot read them. This page lists the ones tied to notifications
and web store messages; record-level ones are on
[Scriptable template synthetic fields: records](scriptable_templates.synthetic_fields_records.md).

The spec gives the field names and the object they belong to, but not their types or exact
contents, so the "Notes" column only says what the name and grouping imply.

## Website

| Field | Notes |
|---|---|
| `customerCenterUrl` | Customer Center address |
| `domain` | Web store domain |
| `url` | Web store address |
| `downloadLink` | Download link (digital delivery) |
| `recoveryPasswordUrl` | Password recovery address |
| `errorticket` | Error ticket reference |
| `cartContents` | Shopping cart contents |

Website notifications also offer `itemurls` and `itemimages`. Both are keyed by the item's internal
ID, so you look up an item's URL or image with that ID.

## Sales order, cash sale and item fulfillment

| Object | Fields |
|---|---|
| Sales order | `trackingLink`, `overviewTable`, `itemAvailabilities`, `licenses` |
| Cash sale | `licenses` |
| Item fulfillment | `trackingLink`, `overviewTable` |

Each entry in `licenses` has `licenseName` and `licenseCode`.

## Cases and issues

| Object | Fields |
|---|---|
| Case notification | `updatedBy`, `messages`, `externalMessages` |
| Issue notification | `url`, `externalStatus`, `externalTarget`, `details` |
| Issue detail (each entry of `details`) | `author`, `date`, `userNote`, `systemNotes` |

## Online forms and campaigns

| Field | Template type |
|---|---|
| `onlineForm.name` | Online form templates |
| `campaignEvent.couponCode` | Campaign email templates |

## Example

A digital-delivery fragment written for this page. The object name `salesorder` follows the
context records listed on the parent page; the loop field names come from the tables above.

```ftl
<#if salesorder.licenses?has_content>
  <ul>
  <#list salesorder.licenses as lic>
    <li>${lic.licenseName}: ${lic.licenseCode}</li>
  </#list>
  </ul>
</#if>
```
