# List records
<!-- id: topic:records_guide.lists -->

Part of: [SuiteScript records guide](records_guide.md)

Sub-topics:

- [Banking records for AI connector access](records_guide.lists.banking.md)

List records are the setup and reference records that other records point to through select fields:
accounts, periods, classifications, currencies, tax setup, price levels, roles, revenue rules and
similar.

## Record types in this group

The group, as the source lists it, is below. Each ID links to its generated record page.

| Area | Records |
|---|---|
| Accounting setup | [account](../gen/records/account.md), [accountingbook](../gen/records/accountingbook.md), [accountingcontext](../gen/records/accountingcontext.md), [accountingperiod](../gen/records/accountingperiod.md), [allocationschedule](../gen/records/allocationschedule.md), [intercompallocationschedule](../gen/records/intercompallocationschedule.md), [amortizationschedule](../gen/records/amortizationschedule.md), [amortizationtemplate](../gen/records/amortizationtemplate.md), [budgetcategory](../gen/records/budgetcategory.md), [budgetexchangerate](../gen/records/budgetexchangerate.md), [consolidatedexchangerate](../gen/records/consolidatedexchangerate.md), [currency](../gen/records/currency.md), [currencyrate](../gen/records/currencyrate.md), [globalaccountmapping](../gen/records/globalaccountmapping.md), [itemaccountmapping](../gen/records/itemaccountmapping.md), [systemnote](../gen/records/systemnote.md), [term](../gen/records/term.md) |
| Organization | [classification](../gen/records/classification.md), [department](../gen/records/department.md), [location](../gen/records/location.md), [subsidiary](../gen/records/subsidiary.md), [subsidiarysettings](../gen/records/subsidiarysettings.md), [nexus](../gen/records/nexus.md), [workplace](../gen/records/workplace.md), [zone](../gen/records/zone.md), `organizationvlaue` [sic] (see [organizationvalue](../gen/records/organizationvalue.md)) |
| Billing and payments | `billlingaccount` [sic] (see [billingaccount](../gen/records/billingaccount.md)), [billingclass](../gen/records/billingclass.md), [billingratecard](../gen/records/billingratecard.md), [billingschedule](../gen/records/billingschedule.md), [billingrevenueevent](../gen/records/billingrevenueevent.md), [automatedclearinghouse](../gen/records/automatedclearinghouse.md), [financialinstitution](../gen/records/financialinstitution.md), [formatprofile](../gen/records/formatprofile.md), [generaltoken](../gen/records/generaltoken.md), [paymentcard](../gen/records/paymentcard.md), [paymentcardtoken](../gen/records/paymentcardtoken.md), [paymentmethod](../gen/records/paymentmethod.md), [giftcertificate](../gen/records/giftcertificate.md), `transactionmatch` (no record page; see [Banking records](records_guide.lists.banking.md)) |
| Pricing | [pricelevel](../gen/records/pricelevel.md), [priceplan](../gen/records/priceplan.md), [pricerule](../gen/records/pricerule.md), [priceruleentry](../gen/records/priceruleentry.md), [pricinggroup](../gen/records/pricinggroup.md), [chargerule](../gen/records/chargerule.md), [fairvaluepricelist](../gen/records/fairvaluepricelist.md) |
| Revenue and projects | [revrecfieldmapping](../gen/records/revrecfieldmapping.md), [revrecplan](../gen/records/revrecplan.md), [revrecschedule](../gen/records/revrecschedule.md), [revrectemplate](../gen/records/revrectemplate.md), [fixedamountprojectrevenuerule](../gen/records/fixedamountprojectrevenuerule.md), [laborbasedprojectrevenuerule](../gen/records/laborbasedprojectrevenuerule.md), [pctcompleteprojectrevenuerule](../gen/records/pctcompleteprojectrevenuerule.md), [projectexpensetype](../gen/records/projectexpensetype.md), [wbs](../gen/records/wbs.md) |
| Items and inventory | [bin](../gen/records/bin.md), [bom](../gen/records/bom.md), [bomrevision](../gen/records/bomrevision.md), [costcategory](../gen/records/costcategory.md), [globalinventoryrelationship](../gen/records/globalinventoryrelationship.md), [inboundshipment](../gen/records/inboundshipment.md), [inventorynumber](../gen/records/inventorynumber.md), [itemcollection](../gen/records/itemcollection.md), [itemcollectionmap](../gen/records/itemcollectionmap.md), [itemlocationconfiguration](../gen/records/itemlocationconfiguration.md), [itemprocessfamily](../gen/records/itemprocessfamily.md), [itemprocessgroup](../gen/records/itemprocessgroup.md), [itemrevision](../gen/records/itemrevision.md), [manufacturingcosttemplate](../gen/records/manufacturingcosttemplate.md), [manufacturingrouting](../gen/records/manufacturingrouting.md), [merchandisehierarchylevel](../gen/records/merchandisehierarchylevel.md), [merchandisehierarchynode](../gen/records/merchandisehierarchynode.md), [merchandisehierarchyversion](../gen/records/merchandisehierarchyversion.md), [supplychainsnapshot](../gen/records/supplychainsnapshot.md), [unitstype](../gen/records/unitstype.md) |
| Tax | [salestaxitem](../gen/records/salestaxitem.md), [taxacct](../gen/records/taxacct.md), [taxgroup](../gen/records/taxgroup.md), [taxperiod](../gen/records/taxperiod.md), [taxtype](../gen/records/taxtype.md) |
| Entities and relationships | [contactcategory](../gen/records/contactcategory.md), [contactrole](../gen/records/contactrole.md), [customercategory](../gen/records/customercategory.md), [customermessage](../gen/records/customermessage.md), [customersubsidiaryrelationship](../gen/records/customersubsidiaryrelationship.md), [entitygroup](../gen/records/entitygroup.md), [othernamecategory](../gen/records/othernamecategory.md), [partnercategory](../gen/records/partnercategory.md), [vendorcategory](../gen/records/vendorcategory.md), [vendorsubsidiaryrelationship](../gen/records/vendorsubsidiaryrelationship.md), [notetype](../gen/records/notetype.md), [ordertype](../gen/records/ordertype.md) |
| People and HR | [employeechangerequest](../gen/records/employeechangerequest.md), [employeechangerequesttype](../gen/records/employeechangerequesttype.md), [driverslicense](../gen/records/driverslicense.md), [othergovernmentissuedid](../gen/records/othergovernmentissuedid.md), [passport](../gen/records/passport.md), [goal](../gen/records/goal.md), [kudos](../gen/records/kudos.md), [payrollitem](../gen/records/payrollitem.md), [performancemetric](../gen/records/performancemetric.md), [performancereview](../gen/records/performancereview.md), [performancereviewschedule](../gen/records/performancereviewschedule.md), [role](../gen/records/role.md), [salesrole](../gen/records/salesrole.md) |

The grouping into areas is this page's own; the source gives a single list.

> Two IDs in the source's list are misspelt versions of real record IDs: `billlingaccount` [sic]
> for `billingaccount`, and `organizationvlaue` [sic] for `organizationvalue`. Both are flagged on
> their generated record pages; use the correctly spelt IDs from those pages, and verify them in
> your account.

`transactionmatch` is in this group but has no record data in the source; what is known about it is
on [Banking records for AI connector access](records_guide.lists.banking.md).

Scripting support (client, server, operations) differs per record; check each record page and the
[SuiteCloud record support catalog](../gen/catalogs/suitecloud_record_support.md).

## Pricing for subscriptions

Among these, [priceplan](../gen/records/priceplan.md) carries the price tiers used by SuiteBilling
subscriptions. It works together with [pricebook](../gen/records/pricebook.md), which collects the
price plans for the items of one subscription plan. Both need SuiteBilling enabled; their record
pages cover intervals, tiers, proration and the rest of their behaviour.
