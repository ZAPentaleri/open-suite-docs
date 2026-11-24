# SuiteFlow email action
<!-- id: topic:suiteflow.send_email -->

Part of: [SuiteFlow workflows](suiteflow.md)

Related: [SuiteFlow action types](suiteflow.actions.md), [SuiteFlow form and field actions](suiteflow.field_actions.md),
[N/email](../modules/N_email.md).

The Send Email action lets a workflow send mail without any script. According to
[SuiteFlow action types](suiteflow.actions.md) it runs on Entry/Exit (its default), After Record
Submit or Scheduled, so you can time a message either with the action's own trigger or by placing it
in a state reached through a scheduled transition.

## Sender and recipients

| Setting | Options |
|---|---|
| Sender | A specific sender, or the value of a field |
| Recipient | The current record, a specific recipient, a free-form address, or the value of a field |

- When you list several addresses, separate them with commas and leave out spaces.
- CC and BCC each accept at most 1,000 characters.

## Message content

The body can be built from an email template, written as custom text, or consist of a link to view
the record. Using email templates requires the `"Mail Merge"` feature (see
[catalog:features](../gen/catalogs/features.md)).

Field values can be merged into CC, BCC and the body with the `{field_id}` notation. To reach a field
on a joined record, put a dot between the join field and the target field, for example
`{<join field>.<field_id>}`.

## Attachments

| Option | What it attaches |
|---|---|
| File | A file from the File Cabinet |
| From field | A file reached through a joined file field |
| Include transaction | The transaction; offered when the base record is any transaction |
| Include statement | A statement; offered when the base record is a customer, lead or prospect |

- An attachment can be at most 10 MB.
- Files stored in image fields cannot be attached.
- To send several files, compress them into one zip file.
- A statement can be placed inline above the message, inline below it, or attached in the default
  format, as HTML or as PDF.

> [!SOURCE-ERROR] In the source, the include-statement options are linked to the parameter
> description of the campaign email action rather than to Send Email. The options listed above are
> given under Send Email; confirm them in the workflow editor before relying on that linked text.
<!-- source-error: topic:suiteflow.send_email | - | statement_options_linked_to_campaign_email_parameters -->

> [!SOURCE-ERROR] The include-transaction options are linked the same way, to the campaign email
> parameters. Treat them as Send Email options, and verify them in the workflow editor.
<!-- source-error: topic:suiteflow.send_email | - | transaction_options_linked_to_campaign_email_parameters -->
