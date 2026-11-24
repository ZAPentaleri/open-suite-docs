# Transactions-related records
<!-- id: topic:records_guide.transactions_related -->

Part of: [SuiteScript records guide](records_guide.md)

This group gathers records that support or describe transaction activity: journal-style entries,
payments and bills, inventory and manufacturing detail, subscriptions and usage, time tracking, and
the generic transaction records.

## Effect on transaction-line volume

Records in this group do not count toward the monthly total of transaction lines an account
generates. Creating or updating them from a script therefore does not add to that monthly volume.

## Record types in this group

| Area | Records |
|---|---|
| Generic and accounting | [transaction](../gen/records/transaction.md), [accountingtransaction](../gen/records/accountingtransaction.md), [customtransaction](../gen/records/customtransaction.md), [memdoc](../gen/records/memdoc.md), [glnumberingsequence](../gen/records/glnumberingsequence.md), [balancetrxbysegments](../gen/records/balancetrxbysegments.md), [unlockedtimeperiod](../gen/records/unlockedtimeperiod.md) |
| Journal entries | [journalentry](../gen/records/journalentry.md), [intercompanyjournalentry](../gen/records/intercompanyjournalentry.md), [advintercompanyjournalentry](../gen/records/advintercompanyjournalentry.md), [statisticaljournalentry](../gen/records/statisticaljournalentry.md) |
| Payables, receivables and cards | [vendorbill](../gen/records/vendorbill.md), [vendorcredit](../gen/records/vendorcredit.md), [vendorpayment](../gen/records/vendorpayment.md), [customerpayment](../gen/records/customerpayment.md), [creditcardcharge](../gen/records/creditcardcharge.md), [creditcardrefund](../gen/records/creditcardrefund.md) |
| Inventory, fulfilment and manufacturing | [inventorydetail](../gen/records/inventorydetail.md), [intercompanytransferorder](../gen/records/intercompanytransferorder.md), [landedcost](../gen/records/landedcost.md), [binworksheet](../gen/records/binworksheet.md), [pickstrategy](../gen/records/pickstrategy.md), [picktask](../gen/records/picktask.md), [orderschedule](../gen/records/orderschedule.md), [itemdemandplan](../gen/records/itemdemandplan.md), [itemsupplyplan](../gen/records/itemsupplyplan.md), [manufacturingoperationtask](../gen/records/manufacturingoperationtask.md), [mfgplannedtime](../gen/records/mfgplannedtime.md) |
| Billing, subscriptions and usage | [charge](../gen/records/charge.md), [subscription](../gen/records/subscription.md), [subscriptionline](../gen/records/subscriptionline.md), [subscriptionchangeorder](../gen/records/subscriptionchangeorder.md), [usage](../gen/records/usage.md) |
| Time | [timebill](../gen/records/timebill.md), [timesheet](../gen/records/timesheet.md) |

The grouping into areas is this page's own; the source gives a single list of 35 records.

> [!SOURCE-ERROR] The source's list of this group shows the charge record twice. It is one record,
> `charge`; treat the second entry as a duplicate and use the record page for its details.
<!-- source-error: topic:records_guide.transactions_related | - | charge_entry_listed_twice -->

Scripting support (client, server, operations) differs per record; see each record page and the
[SuiteCloud record support catalog](../gen/catalogs/suitecloud_record_support.md).
