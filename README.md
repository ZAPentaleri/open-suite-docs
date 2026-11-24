# open-suite-docs

An open, redistributable reference for NetSuite SuiteScript, written for AI code generators and
the developers who review their output.

It covers the SuiteScript 2.1 modules and script types, the concepts around them (governance,
record modes, deployment, SuiteQL, saved searches, templates, CSV import, SDF, SuiteFlow, RESTlet
authentication and more), and SuiteScript 1.0 for one purpose only: reading, converting and
minimally repairing legacy scripts.

> **Not affiliated with Oracle or NetSuite.** NetSuite and SuiteScript are trademarks of Oracle
> and/or its affiliates. This project is an independent, original reference; it is not official
> documentation and is not endorsed by Oracle.

## Start here

- [`llms.txt`](llms.txt): index of every page in the [llms.txt](https://llmstxt.org/) format.
  Point your tooling here first.
- [`script-types/`](script-types/): what each script type must export, and what its entry points
  receive.
- [`modules/`](modules/): every `N/*` module member, with signatures.
- [`topics/`](topics/): concept pages that belong to no single API.
- [`globals.md`](globals.md): global objects and functions (`define`, `require`, `JSON`, `log`,
  `util`, `Promise`).
- [`conversion/ss1-to-ss21.md`](conversion/ss1-to-ss21.md): the full SuiteScript 1.0 → 2.1
  mapping table and migration patterns.
- [`ss1/`](ss1/): SuiteScript 1.0 reference, for legacy code only.
- [`gen/`](gen/index.md): mechanically generated tables (record types, catalogs, and
  SuiteAnalytics Connect tables).

## Page conventions

Pages are written to be parsed as well as read:

- **Symbol markers.** Every documented symbol has an HTML comment directly under its heading,
  for example `<!-- id: N/record.create -->`, carrying a stable identifier.
- **Parameter tables.** Every callable with parameters has a
  `| Parameter | Type | Required | Default |` table, with options objects flattened
  (`options.type`), followed by Returns, Errors, Governance (usage units) and availability.
- **Governance "assumed free".** Where the underlying material gives no usage-unit figure, the page
  says "not documented; assumed to be free (0 units)". Treat that as a convention, not a measured
  value.
- **Source errors.** The material this reference was built from contains mistakes: misspelt
  identifiers, copied or contradictory text. They are kept as found and flagged, never silently
  fixed:
  - ` [sic]` follows a name that looks wrong but may be what the API really uses;
  - a `> [!SOURCE-ERROR]` callout explains what seems wrong and what to verify, followed by a
    machine-readable `<!-- source-error: ... -->` marker.

  Treat flagged details as unconfirmed and test them in your account.
- **Examples.** SuiteScript 2.1, AMD `define()` modules. SuiteScript 1.0 snippets appear only to
  illustrate legacy code being read or converted.

## How it was made

This reference was written in a clean-room process. The facts were first extracted into a
structured specification with no prose. The pages were then written from that specification
alone, in original wording and with original examples, and checked for completeness against it.
Where the specification is silent, the pages say so instead of guessing.

## Status and limitations

- The code examples were reviewed by reading. They have not been executed against a NetSuite
  account. Some examples rely on details the source material does not fully specify, and those
  pages say so.
- Version numbers ("Since") are given only where the source material states them.
- Contributions that correct facts should cite the behaviour observed in a NetSuite account.

## License

- Documentation (all prose and tables): [Creative Commons Attribution 4.0 International](LICENSE)
  (CC BY 4.0).
- Code examples inside the documentation: [MIT](LICENSE-CODE), so you can paste them into your
  own scripts without attribution requirements beyond the MIT notice.

See [NOTICE.md](NOTICE.md) for the trademark notice and the copyright and licensing statement.
