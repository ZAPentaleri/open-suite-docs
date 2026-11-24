# SDF translation collections
<!-- id: topic:sdf.translation_collections -->

Part of: [SuiteCloud Development Framework](sdf.md)

See also: [N/translation](../modules/N_translation.md), [SDF deploy file](sdf.deploy_file.md),
[SDF value lists for script objects](sdf.value_lists.md).

A translation collection is a set of translatable strings (terms) that scripts and other objects can
display in the user's language. Scripts read those strings through the
[N/translation](../modules/N_translation.md) module.

## Parts

In an SDF project a translation collection consists of:

1. a `translationcollection` object, which defines the collection and its strings in the default
   language, and
2. optionally, XLIFF files holding translations. One XLIFF file covers exactly one target language.

## The collection object

The collection's script ID starts with `custcollection_`. The object's root element is
`translationcollection`. The source names the elements by path, where `@` marks an XML attribute
and `>` a child element:

| Element path | What it holds |
|---|---|
| `translationcollection@scriptid` | The collection's script ID (attribute of the root element) |
| `translationcollection>name` | The collection's name |
| `translationcollection>defaultlanguage` | The collection's default language |
| `translationcollection>description` | A description of the collection |
| `translationcollection>strings>string` | One element per term |
| `string@scriptid` | The term's script ID (attribute of `string`) |
| `string>defaulttranslation` | The term's text in the default language |
| `string>description` | A description of the term |

- A term's script ID may use only lower-case letters, digits and underscores, and it must be unique
  inside its collection.
- `defaultlanguage` cannot be changed after the collection has been created. The accepted language
  codes are in
  [catalog:sdf_list_translationcollection_defaultlanguage](../gen/catalogs/sdf_list_translationcollection_defaultlanguage.md).

An invented collection for a field-service app:

```xml
<translationcollection scriptid="custcollection_fieldsvc_labels">
  <name>Field service labels</name>
  <defaultlanguage>en-US</defaultlanguage>
  <description>Texts shown on technician visit screens</description>
  <strings>
    <string scriptid="visit_overdue">
      <defaulttranslation>This visit is overdue</defaulttranslation>
      <description>Banner on late work orders</description>
    </string>
    <string scriptid="parts_missing">
      <defaulttranslation>Parts not yet received</defaulttranslation>
      <description>Warning before dispatch</description>
    </string>
  </strings>
</translationcollection>
```

SDF validation checks that every string fits the length limit of the fields that
reference it, and that strings are unique in the default language.

## XLIFF files

An XLIFF translation file must:

- use XLIFF version 1.2,
- be encoded in UTF-8,
- declare its target language, and that target must differ from the source language, and
- sit in a translations folder at the project root (the source's sample path has the form
  `~/Translations/accommodation_es_ES.xlf`).

Files in that folder whose names do not end in `.xlf` are skipped.

Importing a collection object from an account gives you the object only; no XLIFF files are
created.

### Listing XLIFF files in the deploy file

Every XLIFF file must be listed in the [deploy file](sdf.deploy_file.md) at the element path
`deploy>translationimports>path`: inside `deploy`, a `translationimports` element holds one `path`
element per XLIFF file. A path may use the `*` wildcard to match several files at once. The files
are imported in the order the deploy file lists them.

```xml
<deploy>
  <translationimports>
    <path>~/Translations/fieldsvc_labels_de_DE.xlf</path>
    <path>~/Translations/fieldsvc_labels_fr_*.xlf</path>
  </translationimports>
</deploy>
```

This shows only the translation part of the deploy file; its other sections are described on
[SDF deploy file](sdf.deploy_file.md).

## What deploys change in the account

- Strings from an XLIFF file overwrite the matching translations in the account. Nothing detects
  conflicts, so a deploy silently replaces translations that someone edited in the account.
- Taking a translation out of an XLIFF file does not delete it from the account; the account keeps
  the translation it already has.
- In a SuiteApp project, removing a term from the collection removes its strings on the next deploy.
- In an account customization project, removing the collection from the project leaves the
  collection in the account.
- Uninstalling a SuiteApp removes the collections it brought in.
