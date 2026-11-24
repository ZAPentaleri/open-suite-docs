# Saved search definition options
<!-- id: topic:saved_searches.definition -->

Part of: [Saved searches](saved_searches.md)

See also: [N/search](../modules/N_search.md), [Search formulas](search_formulas.md).

This page collects the choices you make when defining a saved search, along with the rules and side
effects attached to them. A script sets the scriptable parts through
[`search.create`](../modules/N_search.md#searchcreateoptions) (`options.id`, `options.title`,
`options.isPublic`) and stores the search with [`Search.save()`](../modules/N_search.md#searchsave).
Most of the options below (audience, highlighting, reminders) live in the saved search form in the UI.

## Criteria and available filters

Available filters are optional filters that whoever views the results can set for themselves.

- The dropdown of an available filter shows at most 10,000 entries, or 1,000 for entity fields. If
  there are more choices than that, the dropdown comes up empty.
- When the criteria already hold a date or period filter, putting a filter of the same kind in the
  available filters region conflicts with it. Keep the date or period condition in one place.
- Switching on expressions in the criteria turns off inline editing in list views that use the search.

## Script ID

If you don't supply one, a saved search receives a script ID made of the `customsearch` prefix and a
number. Changing a saved search's ID requires the `Server SuiteScript` feature to be enabled in the
account. Giving the search a readable ID when a script creates it (through `options.id`) saves renaming
it later.

In global search, `s:` is the prefix that stands for saved searches.

## Making a search public and choosing its audience

- Marking a search public requires the "Publish Search" permission (`LIST_PUBLISHSEARCH`; see the
  [permissions catalog](../gen/catalogs/permissions.md)).
- Saving or deleting a public search also requires "Publish Search".
- Email alert subscriptions are only possible on a public search.

When the audience is limited by both role and department, a user has to match both to see the search.
Every other combination of audience criteria is satisfied when the user matches any one of them.

By default only the search's owner and administrators can edit it. The option that lets the audience
edit extends those rights to the audience.

## Things you cannot change

The lists shown in the Customer Center cannot be redefined with a saved search.

## Reminders

A search set up as a reminder is not shown as one when its results are grouped by a summary type other
than count.

## Highlighting

Highlighting marks result rows that meet conditions you define. It is set up on the highlighting
subtab of the saved search, both for row conditions and for conditions on summary values.

- **Condition filters.** A highlight condition can test a field of the record, a formula, or a field of
  a related record reached through a join. Expressions are allowed in highlight condition filters. A
  row is highlighted when any of the condition's filters matches.
- **Several conditions.** You can add several highlight conditions, each with its own display
  options. When more than one matches a row, the first matching condition wins; later matching
  conditions only fill in options that earlier ones left unset.
- **Display options.** Each condition can set an image, a text color, a background color, bold text,
  and a description shown in the legend.
- **Summary highlighting.** Conditions on summary values use the summary types count, sum, minimum,
  maximum and average. The field must already have a summary type defined, and the summary type chosen
  for highlighting must match the one set for that field on the results subtab.
- **Period comparisons.** When a highlight compares two periods, the "greater than" option highlights
  only one of the periods, while "magnitude greater than" highlights both.

## Example

A script that saves a public search of active support cases under a readable script ID instead of the
default numbered one. Because the search is public, the role running it needs "Publish Search". Field
IDs come from the [supportcase](../gen/records/supportcase.md) page.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/search', 'N/log'], (search, log) => {
  const execute = () => {
    const activeCases = search.create({
      type: search.Type.SUPPORT_CASE,
      id: 'customsearch_active_cases_by_number',
      title: 'Active cases by number',
      isPublic: true,
      filters: [['isinactive', search.Operator.IS, 'F']],
      columns: [
        search.createColumn({ name: 'casenumber', sort: search.Sort.DESC }),
        'title',
        'company'
      ]
    });

    const savedId = activeCases.save();
    log.audit({ title: 'Saved search created', details: `Internal ID ${savedId}` });
  };

  return { execute };
});
```
