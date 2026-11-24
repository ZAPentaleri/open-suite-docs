# MCP Apps
<!-- id: topic:custom_tool.mcp_apps -->

See also: [CustomTool](../script-types/CustomTool.md), [Custom Tools Page](custom_tool.management.md),
[SDF toolset object](sdf.toolset.md), [SuiteCloud Development Framework](sdf.md).

An MCP App lets a [custom tool](../script-types/CustomTool.md) show an interactive interface inside
an AI chat. The interface is rendered sandboxed in the chat client.

## How it works

- An MCP App is a custom tool whose definition carries UI metadata. The tool declares its interface
  through the `_meta.ui.resourceUri` property, which points at an HTML file.
- From inside the chat, the interface can exchange messages with the host and ask the host to run
  tools.
- The UI side is built with the `@modelcontextprotocol/ext-apps` SDK.

The source does not describe the `_meta` object beyond the `ui.resourceUri` property.

## What you need

- A custom tool script file.
- A JSON-RPC schema for the tool that includes the UI metadata.
- A toolset XML object (see [SDF toolset object](sdf.toolset.md)).
- A bundled HTML file for the interface.

### The toolset XML object

The example in the source uses these elements:

- `toolset`, with its `scriptid` attribute
- `name`
- `scriptfile` (the custom tool script file)
- `rpcschema` (the JSON-RPC schema file, which carries the UI metadata)
- `exposetoaiconnector`
- `permissions`, holding `permission` entries, each with a `permkey` and a `permlevel`

This list comes from an example, not from a complete element reference. Which elements are
required, what they accept and their defaults are described on
[SDF toolset object](sdf.toolset.md).

### The HTML file

- It must be bundled and self-contained: it cannot load dependencies or other external resources at
  run time. Bundle the CSS and JavaScript into the single HTML file.
- The UI's source project does not have to be part of the SuiteCloud project.
- Put the file at the path the metadata references: under the SuiteScripts folder in an account
  customization project, or under the SuiteApps folder in a SuiteApp.
- After deployment the files cannot be edited in the File Cabinet; change them only through SDF.

## Security

- Files in the project can be exposed as UI resources, so treat anything in it as potentially
  visible.
- Hidden files are not a security boundary.
- When you need access control, rely on toolset permissions and choose carefully which tools you
  expose.
- Never put secrets in the UI HTML.

## Steps

1. Meet the normal custom tool requirements.
2. Add `_meta.ui.resourceUri` to the tool definition.
3. Bundle the UI into one HTML file.
4. Put the HTML file at the referenced path.
5. Deploy the project.
6. Connect an AI client.
7. Test the tool in that client.

## Design tips

- Keep logic tools separate from UI tools. A logic tool can then run without any interface when no
  user input is needed.
- Give tools clear, unique names.
- Keep secrets out of the UI HTML.
