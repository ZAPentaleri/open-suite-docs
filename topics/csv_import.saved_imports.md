# Saved CSV import maps
<!-- id: topic:csv_import.saved_imports -->

Part of: [CSV import](csv_import.md)

A saved import map stores the field mappings and advanced options of an import so it can be run
again, by people from the UI or by scripts through
[`CsvImportTask`](../modules/N_task.md#csvimporttask-object) (which can only run saved maps).

## The saved imports list

The list shows each map's ID, name, import type, owner, script ID and access level. Access is one of
Private, Shared or Public.

- Only the map's owner or an administrator can delete it.
- A required field that is mapped cannot be removed from a saved map.
- A map can be copied into another account.

## Script ID

Saved map script IDs start with `CUSTIMPORT` (see [jobs](csv_import.jobs.md) for the naming rules).

> [!SOURCE-ERROR] The source contradicts itself about the script ID when you don't enter one: one
> page gives a default built on `CUSTIMPORT`, while the page about saving the mapping says the field
> stays blank. Don't rely on either; set the script ID explicitly when you save a map that scripts
> will reference.
<!-- source-error: topic:csv_import.saved_imports | - | default_script_id_custimport_vs_blank_when_not_entered -->

## Audience

- If you share a map with both roles and departments, a user must match a listed role **and** a
  listed department. Any other combination of audience settings lets in users who match any one of
  them.
- Members of the audience still need the Import CSV File permission (`ADMI_IMPORTCSVFILE`) to use
  the map.

## Translations

The map's name can be translated; each translation is at most 64 characters. Maps uploaded with
SDF cannot be translated in the UI.

## Moving maps between accounts

Saved maps can be deployed with SDF (as a saved CSV import object; see [SDF](sdf.md)) or with
SuiteBundler.

Default values set in a bundled map are not remapped in the target account. Because internal IDs
differ between accounts, a default that points at a record by internal ID may refer to the wrong
record, or to none, after installation. Queue numbers on bundled maps are reset to 1 (see
[threads and queues](csv_import.multithreading.md)).
