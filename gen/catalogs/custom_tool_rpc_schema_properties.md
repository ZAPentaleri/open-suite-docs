# Catalog: custom_tool_rpc_schema_properties
<!-- id: catalog:custom_tool_rpc_schema_properties -->

Used with: `script:CustomTool`

| ID | Label | Value | Tags |
|---|---|---|---|
| `name` |  | string | required, unique_within_schema, method_name_in_script |
| `description` |  | string | required, used_by_ai_client_to_choose_tool |
| `inputSchema` |  | object | required |
| `inputSchema.type` |  | string | required, only_object |
| `inputSchema.properties` |  | object | required, param_types_string_number_boolean |
| `inputSchema.required` |  | array<string> | required |
| `inputSchema.nullable` |  | array<string> | required_for_chatgpt |
| `outputSchema` |  | object | required_for_chatgpt, optional_for_other_clients |
| `outputSchema.type` |  | string | required_for_chatgpt, optional_for_other_clients |
| `outputSchema.properties` |  | object | required |
| `outputSchema.required` |  | array<string> | required |
| `outputSchema.nullable` |  | array<string> | required_for_chatgpt |
| `annotations` |  | object | optional |
| `annotations.title` |  | string | optional, defaults_to_tool_name |
| `annotations.readOnlyHint` |  | boolean | optional, default_false, true_means_no_environment_change |
| `annotations.idempotentHint` |  | boolean | optional, default_false, applies_when_not_read_only |
| `annotations.openWorldHint` |  | boolean | optional, default_false, true_means_external_interaction |
| `annotations.destructiveHint` |  | boolean | optional, default_true, applies_when_not_read_only |
| `_meta.ui.resourceUri` |  | string | optional, mcp_app_only, html_file_in_file_cabinet |
