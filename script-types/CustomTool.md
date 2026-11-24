# CustomTool
<!-- id: script:CustomTool -->

A custom tool script lets an external AI client run your SuiteScript. You write the functions,
describe each one as a "tool" in a JSON-RPC schema, and the NetSuite AI Connector Service exposes
them. When a user types a natural-language request in the AI client, the client reads the tool
definitions, decides which tool applies and chooses its parameter values, and NetSuite runs the
matching function on the server. Tools can retrieve data, trigger actions, and perform most
operations SuiteScript supports elsewhere. Design them on the assumption that the tool's user is an AI
acting on behalf of a customer.

## At a glance

| Item | Value |
|---|---|
| `@NScriptType` | `CustomTool` (required) |
| Usage-unit limit per execution | 1,000 |
| Time limit | 300 seconds |
| Runs on | Server |
| Invoked through | The NetSuite AI Connector Service, prompted by natural-language requests |
| Separate NetSuite login for the AI client | Not required |
| Script record / deployment record | Neither; deployed with SDF as part of a toolset |
| Entry points | None fixed: the `async` methods named in the JSON-RPC schema |
| Modules you cannot use | [N/http](../modules/N_http.md), [N/https](../modules/N_https.md), [N/llm](../modules/N_llm.md), [N/sftp](../modules/N_sftp.md) |

Related pages: [managing custom tools](../topics/custom_tool.management.md),
[MCP apps (interactive UI)](../topics/custom_tool.mcp_apps.md),
[toolset SDF object](../topics/sdf.toolset.md), [SDF](../topics/sdf.md),
[schema properties catalog](../gen/catalogs/custom_tool_rpc_schema_properties.md),
[UserEventScript](UserEventScript.md), [WorkflowActionScript](WorkflowActionScript.md).

## How it runs

**No fixed entry points.** A custom tool has no predefined entry point names; the JSON schema defines
them. Every tool in the schema corresponds to a method your script returns from `define`, under
exactly the tool's name. The script must return every method the schema defines, and each tool
function must be declared `async`. A tool method receives one argument: an object whose keys are the
input property names declared in the tool's input schema.

**What you deploy.** A custom tool never has a script record or a deployment record. You deploy it
with the SuiteCloud Development Framework, in a SuiteApp project or an account customization project.
It needs three things, tied together by a [toolset SDF object](../topics/sdf.toolset.md):

- the custom tool script file;
- the JSON-RPC schema file that describes the tools;
- the permissions a user needs, which are set on the toolset object.

The older tool SDF custom object is deprecated as of 2027.1, and since the September 2026 minor
release tools defined with tool XML definitions can no longer be deployed. Use the toolset object
instead. After deployment you cannot edit the tool files in the File Cabinet; change them only
through SDF. If the script refers to a module that is missing, the deployment fails.

**Who sees a tool.** A tool appears in the AI client only for users who hold every permission listed
on its toolset. What a tool can reach is further limited by the user's role and permissions. When the
role cannot access the File Cabinet folder that holds the tool, the tool call fails.

**Side effects on other customizations.** Records a tool touches trigger other customizations as
usual. User event scripts and workflow action scripts triggered that way run under the AI Connector
Service role, and so do workflows, unless a workflow is set to run as administrator.

**Logs.** For tools defined with a toolset object, execution logs are on the script execution logs
page under Customization > Scripting.

**Modules.** `N/http`, `N/https`, `N/llm` and `N/sftp` are not supported in custom tools.

> [!SOURCE-ERROR] The source's error table names only `N/http`, `N/https` and `N/sftp` as unsupported,
> leaving out `N/llm`, while its other statements include `N/llm`. Treat `N/llm` as unsupported too,
> and confirm with a test deployment if it matters.
<!-- source-error: script:CustomTool | - | error_table_lists_unsupported_modules_without_n_llm -->

### The JSON-RPC schema

The schema file is an object with a `tools` array; each array element is one tool definition. A tool
definition gives a name, a description and the input parameters. The AI client uses the definition
to decide when the tool applies and which parameter values to pass, so the description is what lets
the client pick the right tool. The full list of allowed properties is in the
[schema properties catalog](../gen/catalogs/custom_tool_rpc_schema_properties.md).

> [!SOURCE-ERROR] The source's first schema sample has an extra opening brace, so it is not valid JSON
> as printed. Do not copy it; build the file from the property catalog and check that it parses.
<!-- source-error: script:CustomTool | - | first_schema_sample_has_extra_opening_brace -->

Rules:

- The schema must be valid JSON and may contain only the documented properties. An invalid schema
  makes deployment validation fail.
- Tool names must be unique within the schema and must match the methods the script returns.
- Input properties can be `string`, `number` or `boolean`. Output properties can be `string`,
  `number`, `boolean` or `array`; an array output needs an `items` property.
- When the client is ChatGPT, `outputSchema` and `nullable` are required.
- Annotation hints: `readOnlyHint`, `idempotentHint` and `openWorldHint` default to `false`;
  `destructiveHint` defaults to `true`. `idempotentHint` and `destructiveHint` only apply when
  `readOnlyHint` is `false`. The annotation title defaults to the tool name.
- A tool can provide an interactive UI as an MCP app ([MCP apps](../topics/custom_tool.mcp_apps.md)).
  The UI resource must be an HTML file inside the SuiteApp folder; in a SuiteApp project its URI has
  the form `ui://SuiteApps/<SuiteAppId>/<folder>/<file>.html`.

### Errors you can run into

At run time a tool call fails when the role cannot access the File Cabinet folder, the script uses an
unsupported module, required parameters are missing, the execution context cannot be created, the
session lacks permission for the tool, an exception is thrown while it runs, or the function does not
return a proper result.

SDF validation rejects a deployment when the RPC schema fails validation, the toolset cannot be
prepared, the schema is not valid JSON, it uses an unsupported property, a schema tool has no exported
method, a tool name is duplicated, an input schema is invalid, or an MCP app UI resource is missing,
has an invalid path or is not an HTML file.

### Best practices

- Give every method a unique name, and use a common prefix for all tools from one SuiteApp or
  publisher.
- Write accurate tool descriptions.
- Convert objects to strings (for example with `JSON.stringify`) before returning them.
- Avoid tools whose purposes overlap: overlapping tools make the AI's behaviour unpredictable.
- Expose only operations that are safe, auditable and suitable for automation.
- Keep untrusted data out of tool output.

Sample tools are published in the `MCP-Sample-Tools` folder of the SuiteCloud project repository on
GitHub.

## Examples

A schema with one read-only tool that reports a customer's credit position. The tool name carries a
publisher prefix:

```json
{
  "tools": [
    {
      "name": "acmeCreditSnapshot",
      "description": "Returns the balance, overdue balance and credit limit of one customer, given the customer's internal ID.",
      "inputSchema": {
        "type": "object",
        "properties": {
          "customerId": { "type": "string", "description": "Internal ID of the customer" }
        },
        "required": ["customerId"]
      },
      "annotations": { "title": "Customer credit snapshot", "readOnlyHint": true }
    }
  ]
}
```

The matching script. The method name equals the tool name, the function is `async`, it receives an
object keyed by the input property names, and the result is returned as a string:

```js
/**
 * @NApiVersion 2.1
 * @NScriptType CustomTool
 */
define(['N/search'], (search) => {
  const acmeCreditSnapshot = async (args) => {
    const fields = search.lookupFields({
      type: 'customer',
      id: args.customerId,
      columns: ['entityid', 'balance', 'overduebalance', 'creditlimit']
    });
    return JSON.stringify({
      customer: fields.entityid,
      balance: Number(fields.balance) || 0,
      overdue: Number(fields.overduebalance) || 0,
      creditLimit: Number(fields.creditlimit) || 0
    });
  };

  return { acmeCreditSnapshot };
});
```
