# Transitioning to SuiteScript 2.1
<!-- id: topic:ss21_transition -->

Part of: [SuiteScript platform overview](suitescript_overview.md)

Sub-topics:

- [SuiteScript 1.0 to 2.1 API map](ss21_transition.api_map.md)
- [Converting SuiteScript 1.0 scripts](ss21_transition.from_ss1.md)
- [SuiteScript 2.0 to 2.1 differences](ss21_transition.from_ss20.md)
- [SuiteScript 1.0 conversion samples](ss21_transition.ss1_samples.md)
- [SuiteScript 1.0 conversion samples (searches and imports)](ss21_transition.ss1_samples_more.md)

See also: [Converting SuiteScript 1.0 to 2.1](../conversion/ss1-to-ss21.md) (the full mapping
table and patterns).

This topic covers the retirement timeline, the paths for moving scripts to 2.1, converting 1.0
scripts, converting 2.0 scripts, troubleshooting, and the 1.0 to 2.1 API map.

## Timeline

| Release | What happens |
|---|---|
| 2026.2 | 2.1 becomes the standard version |
| 2027.1 | SuiteScript 1.0 reaches end of life; only critical fixes from then on |
| 2028.1 | 1.0 scripts can no longer be deployed in new accounts, and 2.x scripts run as 2.1 |
| 2028.2 | Every script must be 2.1 |

The versions being retired are `1.0`, `2.0` and `2.x`. Version 2.1 brings `let`, `const`, arrow
functions, the spread operator and `async`/`await`. Where a particular feature has its own version
requirement, follow it.

## Your options for each script

- Update it to 2.1.
- Test it on the 2.1 runtime first, then update it.
- Retire it if nobody needs it any more.

## Finding scripts that still need work

- **Quick review:** in **Customization > Scripting > Scripts**, filter the list by API version.
- **Saved search:** search scripts whose API version is not 2.1, and show name, ID, type, API
  version, owner and description in the results.
- **Detailed inventory:** build a SuiteAnalytics workbook from **Analytics > Workbooks > New Workbook**.

Group what you find by API version or script type to decide what to tackle first, and read each
script's source and dependencies before changing it.

## Planning the move

Across the account:

1. Inventory the scripts.
2. Put the business-critical ones first.
3. Choose an approach per script.
4. Test.
5. Roll out in phases and watch the results.

Per script:

1. Take stock of the script.
2. Identify its version and script type.
3. Choose an option (update, test first, retire).
4. Convert it in a sandbox.
5. Review the code.
6. Validate it.
7. Deploy it.

Start with simple, low-risk scripts. Keep each script on its current version until its
compatibility problems are fixed, and keep its script ID, deployments and dependencies unchanged so
that nothing pointing at it breaks.

To validate a converted script:

- run it in a sandbox,
- exercise every entry point,
- check sublists, searches, record operations and error handling, and
- confirm the deployments still reference the right file.

Compare its behaviour with the original's.

## AI help: SuiteCloud Developer Assistant

The SuiteCloud Developer Assistant can help with conversion. Its migration skill has four modes:
analyze, convert, explain and validate. It finds legacy version annotations, `nlapi`/`nlobj` calls,
1-based sublist indexing and date handling that needs attention.

Use it when a script is annotated `1.0`, `2.0` or `2.x`, when it calls 1.0 APIs, or when you want
to check whether a script is compatible with the 2.1 runtime. A suggested sequence: take stock of
the script, analyze it, test it in a sandbox with the 2.1 preference on, convert it, validate it,
and then test its function in the sandbox. For example, a 1.0 user event script on customers can be
analyzed and converted this way.

- The assistant's validation is a static check of the code, not a functional test.
- Some legacy APIs have no mapping; they need native JavaScript or another approach.
- Review every line of generated code before it goes to production.

## Testing 2.0 and 2.x scripts on the 2.1 runtime

Two account preferences, under **Setup > Company > General Preferences**, run existing server scripts
on the 2.1 runtime without editing them: one for scripts annotated `2.0` and one for scripts
annotated `2.x`.

- They apply to server scripts only, and script types that are not supported are unaffected.
- Use them to test 2.0 and 2.x server scripts on the 2.1 runtime. They can be switched back to 2.0
  at any time.
- They do not change upload validation: a file annotated `2.x` is still checked against 2.0 syntax
  rules when you upload it, so saving 2.1-only syntax in it raises a syntax error. The preference
  does not let you save 2.1 syntax in a 2.x script.
- A `2.x` script containing 2.1 syntax fails when the preference is set to 2.0.
- Fix either way: annotate the script `2.1` explicitly to use 2.1 syntax, or, while the preference
  is on 2.0, remove the 2.1 syntax.

A script has passed validation when it runs with the preference on, raises no validation error,
behaves as expected, and leaves no 2.1-related errors in the logs. Once it passes, annotate it as
`2.1`.
