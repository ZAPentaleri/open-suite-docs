# Text Enhance Action (`textenhanceaction`)
<!-- id: record:textenhanceaction -->

| Fact | Value |
|---|---|
| Internal ID | `textenhanceaction` |
| Category | customization |
| Client scripts | yes |
| Server scripts | yes |
| Searchable | yes |
| Tags | `no_transform`, `supports_copy`, `supports_create`, `supports_delete`, `supports_read`, `supports_search`, `supports_update` |

## Record facts

- `script_types`: `client`, `server`
- `script_functions`: `create`, `update`, `delete`
- `related` (refs `N/llm`)
- `applies_to_fields`: `long_text`, `text_area`, `rich_text`
- `raises_error`: `save_error` (when `create_only_field_changed`)
- `purpose`: `text_enhance_action_on_text_fields`
- `contains`: `name`, `script_id`, `language`, `partial_action_flag`
- `ui_location`: `prompt_studio`
