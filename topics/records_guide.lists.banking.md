# Banking records for AI connector access
<!-- id: topic:records_guide.lists.banking -->

Part of: [List records](records_guide.lists.md)

Four banking records exist mainly so that AI clients connected through the AI connector (MCP) can
read bank data and work with reconciliation matches. The source documents none of them as a full
record (no field list, field types, search filters or search columns), so this reference has no
record page for any of them; check field IDs in your account's Records Browser. None of them has a
standard record form in the UI.

## At a glance

| Record ID | Script side | SuiteScript version | Operations | Permission |
|---|---|---|---|---|
| `bankrecsuggestion` | Server only | 2.1 | Read | Reconcile at View level (actions need more, see below) |
| `transactionmatch` | Server only | 2.1 | Read, create | Reconcile: View to read, Create to create |
| `bankingtransaction` | Server only | 2.1 | Read | Reconcile at View level |
| `bankingstatement` | Server only | 2.1 | Read | Reconcile at View level |

The Reconcile permission (`TRAN_RECONCILE`) and its levels are listed in the
[permissions catalog](../gen/catalogs/permissions.md).

## Bank reconciliation suggestions (`bankrecsuggestion`)

A bank reconciliation suggestion is a match the system proposes. Server scripts can read it; there
is no standard form for it, and the usual account, subsidiary and transaction restrictions limit
what a user sees. Its details sit in three subrecords: `autoapplysuggestiondetail`,
`automatchsuggestiondetail` and `suggestionbankingtransaction`. Fields that the source shows in an
example include `suggestiontype`, `suggestionsource` and `isagentrecommended`; the source does not
give their types or meanings.

### Accept and reject actions

Two record actions are defined on the suggestion:

- **`accept`** submits the suggested match for reconciliation. It needs the Reconcile permission at
  Create level.
- **`reject`** turns the suggestion down. It needs the Reconcile permission at Edit level.

Facts about these actions:

- Invoke them with [`action.execute`](../modules/N_action.md) (module [N/action](../modules/N_action.md)).
  Each call returns an object with `notifications` (errors and warnings) and `response` (the
  action's result).
- They can be called from both client and server scripts, even though reading the record itself is
  server only. They are available since version 2026.2.0.13.
- The only parameter is the record's ID. Each call acts on one suggestion; running an action on all
  qualifying instances at once is not supported.
- In the UI, suggestions appear on the Match Suggestions subtab of the Match Bank Data page. The
  Submit and Reject buttons there do not go through these record actions.
- AI clients use the same actions to accept or reject suggestions through the AI connector.

## Transaction matches (`transactionmatch`)

A transaction match links imported bank transactions to general ledger transactions. Server scripts
written in SuiteScript 2.1 can read and create matches. The account must use Match Bank Data, and
account restrictions apply as usual. AI clients can also work with matches.

- Two subrecords carry the two sides: `generalledgertransactionmatch` and
  `importedtransactionmatch`.
- Sublists can be filled only while the match is being created. You cannot create child match
  entries on their own, and you cannot update an existing match.
- These fields are read-only: `automatch`, `matchtype`, `matchsource`, `suggestedbysystem`,
  `matchedby` and `reconciledby`.
- The transaction line ID holds the line's ID, not the line's position (index) in a sublist.
- There is no standard record form.

## Banking transactions and statements

A banking transaction (`bankingtransaction`) is an imported bank line. It holds the account, the
transaction date, the amount, the payee and the memo.

A banking statement (`bankingstatement`) holds the account, the as-of date, the opening balance and
the balance.

Both can only be read, from server scripts in SuiteScript 2.1, with the Reconcile permission at View
level. The source names these contents in words and does not give their field IDs.

## Example

A small server-side helper that rejects one bank reconciliation suggestion and logs what came back.
The running role needs Reconcile at Edit level; the suggestion ID is supplied by the caller.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/action', 'N/log'], (action, log) => {
  const rejectSuggestion = (suggestionId) => {
    const outcome = action.execute({
      recordType: 'bankrecsuggestion',
      id: 'reject',
      params: { recordId: Number(suggestionId) }
    });
    if (outcome.notifications && outcome.notifications.length > 0) {
      log.error({ title: `Reject ${suggestionId}`, details: JSON.stringify(outcome.notifications) });
      return false;
    }
    log.audit({ title: `Rejected ${suggestionId}`, details: JSON.stringify(outcome.response) });
    return true;
  };

  return { rejectSuggestion };
});
```

## What the source does not state

The source gives no field types and no search filters or columns for any of the four records, and
no field IDs for the contents of banking transactions and statements.
