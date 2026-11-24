# Custom Tools Page
<!-- id: topic:custom_tool.management -->

See also: [CustomTool](../script-types/CustomTool.md), [MCP Apps](custom_tool.mcp_apps.md),
[N/file](../modules/N_file.md).

The Custom Tools page is where an account manages the [custom tools](../script-types/CustomTool.md)
installed in it. Open it from Customization > Scripting > Custom Tools.

## What the page shows

- Every tool that comes from a SuiteApp or from an account customization project (ACP).
- Tools grouped by toolset.
- The permissions each tool needs.

## Who can do what

| Task | Requirement |
|---|---|
| View the page, delete toolsets | SuiteScript permission at Full |
| Activate or deactivate toolsets, manage file access | Administrator role |

## Activation

- You activate or deactivate a whole toolset, never an individual tool.
- A tool is available only while its toolset is active and its exposure and permission requirements
  are met.
- When a toolset is deactivated, its tools can no longer be discovered or run.

## Deleting toolsets

- Toolsets that came from an ACP can be deleted here. Deleting a toolset deletes every tool in it.
- Toolsets that came from a SuiteApp cannot be deleted here. To remove them, uninstall the SuiteApp.

## File Cabinet access list

The page holds one list of File Cabinet folders that applies to all custom tools in the account.

### What it controls

- The list governs what [N/file](../modules/N_file.md) can reach while a custom tool is running.
- An empty list blocks access to every File Cabinet file.
- It does not filter folder listings, searches by file name, lookups by path, or files attached to
  records through other modules.

### How entries combine

- An entry covers the folder and all its subfolders.
- Where entries overlap, the highest access level wins. An entry for a subfolder therefore cannot narrow
  what an entry for its parent folder grants.
- The list cannot grant more than the role's own permissions allow.

### Access levels

| Level | Allows |
|---|---|
| View (default) | Reading files |
| Create | Reading and creating files |
| Edit | Reading, creating, updating and replacing files |

### Limits and rules

- At most 1,000 folders.
- The File Cabinet root cannot be added, and the same folder cannot be added twice.
- Changes are saved immediately.
- Give tools a dedicated folder outside SuiteScripts. Granting access to SuiteScripts would expose your
  scripts to tools, or let tools change them.
