# N/llm
<!-- id: N/llm -->

`N/llm` is the module a server script uses to send generative AI requests to large language
models that NetSuite hosts. A script can:

- generate text from a free-form prompt, optionally with earlier chat turns and a preamble;
- run a prompt saved in Prompt Studio, filling in its variables;
- ground an answer in documents you supply (retrieval-augmented generation, RAG) and get
  citations back;
- ask questions about an image with the Cohere Command A Vision model;
- stream a response, reading partial text before generation finishes;
- let the model call tools that your script implements;
- turn text into embeddings (vectors);
- re-rank a list of documents by relevance to a prompt.

Requests consume NetSuite AI units, an allowance separate from script governance. Read the
remaining balance with [`llm.getRemainingUsage()`](#llmgetremainingusage). The older
`getRemainingFree...` functions are deprecated.

- **Availability:** server scripts only. The module requires the feature `"Server SuiteScript"`,
  and it works only in the regions where the service is offered.
- **Load it as:** `define(['N/llm'], (llm) => { ... })`
- **Older names.** The source lists alternative names that behave the same: `llm.chat` for
  `generateText`, `llm.executePrompt` for `evaluatePrompt`, `llm.chatStreamed` for
  `generateTextStreamed` and `llm.executePromptStreamed` for `evaluatePromptStreamed` (each with a
  matching `.promise` form).
- **Treat output with care.** Generated content can be wrong, so validate it before relying on it
  or storing it. Embeddings carry the same sensitivity as the text they came from; protect them
  the same way.
- **Embedding inputs** longer than 512 tokens are handled according to the
  [`llm.Truncate`](#llmtruncate) value you pass.
- Related: [Governance](../topics/governance.md), [N/record](N_record.md),
  [Iterator](../globals.md#iteratorcallback),
  [model parameter ranges](../gen/catalogs/llm_model_parameter_ranges.md).

### Concurrency

Two separate counters apply. Up to 5 generation requests (`generateText`, `generateTextStreamed`,
`evaluatePrompt`, `evaluatePromptStreamed` and their promise forms) may be in flight at once, and,
counted independently, up to 5 embedding requests (`embed`, `embed.promise`). A request beyond
the limit fails with `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`. `reRank` reports its own error,
`MAXIMUM_PARALLEL_RE_RANK_REQUESTS_LIMIT_EXCEEDED`; the source does not state how many re-rank
requests may run in parallel.

### Prompts and Text Enhance actions as records

Prompt Studio is where prompts are managed. Saved prompts and Text Enhance actions are also
records (`prompt` and `textenhanceaction`) that a script can create, load, change and delete with
[N/record](N_record.md); deleting uses `record.delete`.

- A record's script ID is generated automatically when it is saved. Load an existing record by
  its numeric internal ID.
- A `prompt` record requires `name`, `prompttype`, `modelfamily` and `template`. A
  `textenhanceaction` record requires `name` and `language`.
- In Prompt Studio the required fields carry a star, and clicking a field reveals its ID. Prompt
  Studio is also where you look up a prompt's internal ID or script ID.

### Tool calling workflow

Tools let the model ask your script to run code (a lookup, a calculation) and then use the
outcome. Only `generateText` and `generateTextStreamed` accept tools. One round trip:

1. Describe each argument with [`llm.createToolParameter`](#llmcreatetoolparameteroptions).
2. Build each tool with [`llm.createTool`](#llmcreatetooloptions).
3. Pass the tools to `generateText` (or `generateTextStreamed`).
4. For every entry in the response's `toolCalls`, run your handler for that tool.
5. Wrap each handler's outcome with [`llm.createToolResult`](#llmcreatetoolresultoptions).
6. Call `generateText` again with `toolResults` and the updated `chatHistory`. The prompt may be
   omitted on that call.

Guidance the source gives:

- Repeat steps 4 to 6 until a response arrives with no tool calls.
- Carry the chat history from each response into the next call.
- Give tools meaningful names and descriptions; the model relies on them to choose a tool.
- The parameter names your handler reads must match the names in the tool definition.
- Keep handler results simple, ideally a primitive value.
- When a handler fails, return a flag value that tells the model so, rather than throwing.
- Check data access inside handlers; the model picks the arguments, so do not trust them blindly.
- Too many tools hurt both performance and how clearly the model chooses. The tool set may differ
  from one call to the next.

## Members

| Member | Kind | Summary |
|---|---|---|
| [`llm.generateText(options)`](#llmgeneratetextoptions) | function | Send a prompt (with optional history, documents, image, tools) and get a response |
| [`llm.generateText.promise(options)`](#llmgeneratetextpromiseoptions) | function | Promise form of `generateText` |
| [`llm.generateTextStreamed(options)`](#llmgeneratetextstreamedoptions) | function | Like `generateText`, but readable while it is produced |
| [`llm.generateTextStreamed.promise(options)`](#llmgeneratetextstreamedpromiseoptions) | function | Promise form of `generateTextStreamed` |
| [`llm.evaluatePrompt(options)`](#llmevaluatepromptoptions) | function | Run a saved Prompt Studio prompt |
| [`llm.evaluatePrompt.promise(options)`](#llmevaluatepromptpromiseoptions) | function | Promise form of `evaluatePrompt` |
| [`llm.evaluatePromptStreamed(options)`](#llmevaluatepromptstreamedoptions) | function | Run a saved prompt with a streamed response |
| [`llm.evaluatePromptStreamed.promise(options)`](#llmevaluatepromptstreamedpromiseoptions) | function | Promise form of `evaluatePromptStreamed` |
| [`llm.embed(options)`](#llmembedoptions) | function | Turn text inputs into embedding vectors |
| [`llm.embed.promise(options)`](#llmembedpromiseoptions) | function | Promise form of `embed` |
| [`llm.reRank(options)`](#llmrerankoptions) | function | Order documents by relevance to a prompt |
| [`llm.reRank.promise(options)`](#llmrerankpromiseoptions) | function | Promise form of `reRank` |
| [`llm.createChatMessage(options)`](#llmcreatechatmessageoptions) | function | Build a chat history entry |
| [`llm.createDocument(options)`](#llmcreatedocumentoptions) | function | Build a source document for RAG |
| [`llm.createToolParameter(options)`](#llmcreatetoolparameteroptions) | function | Describe one argument of a tool |
| [`llm.createTool(options)`](#llmcreatetooloptions) | function | Define a tool the model may call |
| [`llm.createToolResult(options)`](#llmcreatetoolresultoptions) | function | Wrap a tool's output for the model |
| [`llm.getRemainingUsage()`](#llmgetremainingusage) | function | Remaining AI units |
| [`llm.getRemainingUsage.promise()`](#llmgetremainingusagepromise) | function | Promise form of `getRemainingUsage` |
| [`llm.getRemainingFreeUsage()`](#llmgetremainingfreeusage) | function | Deprecated; use `getRemainingUsage` |
| [`llm.getRemainingFreeUsage.promise()`](#llmgetremainingfreeusagepromise) | function | Deprecated promise form |
| [`llm.getRemainingFreeEmbedUsage()`](#llmgetremainingfreeembedusage) | function | Deprecated; use `getRemainingUsage` |
| [`llm.getRemainingFreeEmbedUsage.promise()`](#llmgetremainingfreeembedusagepromise) | function | Deprecated promise form |
| [`Response`](#response-object) | object | Result of a non-streamed generation |
| [`StreamedResponse`](#streamedresponse-object) | object | Result of a streamed generation |
| [`Usage`](#usage-object) | object | Token counts of a response |
| [`ChatMessage`](#chatmessage-object) | object | One entry in a chat history |
| [`Document`](#document-object) | object | A source document for RAG |
| [`Citation`](#citation-object) | object | A stretch of the answer tied to source documents |
| [`Tool`](#tool-object) | object | A tool definition |
| [`ToolParameter`](#toolparameter-object) | object | One argument of a tool |
| [`ToolCall`](#toolcall-object) | object | The model's request to run a tool |
| [`ToolResult`](#toolresult-object) | object | A tool's output, sent back to the model |
| [`EmbedResponse`](#embedresponse-object) | object | Result of `embed` |
| [`ReRankResponse`](#rerankresponse-object) | object | Result of `reRank` |
| [`DocumentRank`](#documentrank-object) | object | One ranked document |
| [`llm.ModelFamily`](#llmmodelfamily) | enum | Models for text generation |
| [`llm.EmbedModelFamily`](#llmembedmodelfamily) | enum | Models for embeddings |
| [`llm.ReRankModelFamily`](#llmrerankmodelfamily) | enum | Models for re-ranking |
| [`llm.SafetyMode`](#llmsafetymode) | enum | Content safety level for Cohere models |
| [`llm.ReasoningEffort`](#llmreasoningeffort) | enum | Reasoning depth for the GPT-OSS model |
| [`llm.ChatRole`](#llmchatrole) | enum | Author of a chat message |
| [`llm.Truncate`](#llmtruncate) | enum | How over-long embedding inputs are cut |
| [`llm.ToolParameterType`](#llmtoolparametertype) | enum | Data type of a tool argument |

## Functions

### llm.generateText(options)
<!-- id: N/llm.generateText -->

Sends a prompt to a model and waits for the complete answer. Alongside the prompt you can pass
earlier turns, a preamble, documents for RAG, an image (vision model only), tools the model may
request, the results of earlier tool calls, and a JSON schema the answer must follow. Every call
consumes AI units. The source also lists the name `llm.chat` for this function.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.prompt` | string | No | |
| `options.documents` | llm.Document[] | No | |
| `options.image` | file.File | No | |
| `options.modelFamily` | string | No | |
| `options.modelParameters` | Object | No | |
| `options.modelParameters.reasoningEffort` | string | No | |
| `options.modelParameters.topP` | number | No | |
| `options.modelParameters.presencePenalty` | number | No | |
| `options.modelParameters.maxTokens` | number | No | |
| `options.modelParameters.temperature` | number | No | |
| `options.modelParameters.frequencyPenalty` | number | No | |
| `options.modelParameters.topK` | number | No | |
| `options.ociConfig` | Object | No | |
| `options.responseFormat` | Object | No | |
| `options.safetyMode` | string | No | `llm.SafetyMode.STRICT` |
| `options.timeout` | number | No | `30000` |
| `options.toolResults` | llm.ToolResult[] | No | |
| `options.tools` | llm.Tool[] | No | |
| `options.chatHistory` | llm.ChatMessage[] | No | |
| `options.preamble` | string | No | |

- `options.prompt`: conditionally required. Supply it unless you pass `options.toolResults`; when
  tool results are present the prompt is ignored. Since 2024.1.
- `options.documents`: source documents for RAG, built with
  [`llm.createDocument`](#llmcreatedocumentoptions). Only Cohere models accept them, and every
  document ID must be unique. Since 2025.1.
- `options.image`: a file for the model to examine. Only Cohere Command A Vision takes it. Suited
  to writing captions, describing an image or reading figures off a chart. Since 2026.1.
- `options.modelFamily`: a [`llm.ModelFamily`](#llmmodelfamily) value; `COHERE_COMMAND` (model
  `cohere.command-a-03-2025`) when omitted. Since 2024.2.
- `options.modelParameters`: tuning values whose allowed ranges depend on the model; see
  [llm_model_parameter_ranges](../gen/catalogs/llm_model_parameter_ranges.md). As a rule of thumb
  one word averages about 3 tokens. Since 2024.1.
  - `reasoningEffort`: a [`llm.ReasoningEffort`](#llmreasoningeffort) value, `MEDIUM` when
    omitted. Only the OpenAI GPT-OSS model supports it. More effort means more tokens and longer
    waits. Since 2026.2.
  - `topP`: applied after `topK`. Since 2024.1.
  - `presencePenalty`: on Cohere Command models it cannot be combined with `frequencyPenalty`.
    Since 2024.1.
  - `maxTokens`: caps the answer length. At roughly 3 tokens per word, a higher cap allows a
    longer answer, which can cost more AI units. Since 2024.1.
  - `temperature`: low for factual answers, higher for creative ones. Since 2024.1.
  - `frequencyPenalty`, `topK`: nothing beyond their type and the ranges catalog. Since 2024.1.
- `options.ociConfig`: no longer supported and always ignored. Since 2024.1.
- `options.responseFormat`: a JSON schema; the model then answers with structured JSON that
  follows it. It cannot be combined with `documents`, `tools` or `toolResults`. Since 2025.1.
- `options.safetyMode`: a [`llm.SafetyMode`](#llmsafetymode) value; `STRICT` when omitted. Since
  2025.1.
- `options.timeout`: in milliseconds. Since 2024.1.
- `options.toolResults`: outcomes of earlier tool calls, built with
  [`llm.createToolResult`](#llmcreatetoolresultoptions). When present, the prompt is ignored. Not
  supported by Cohere Command A Vision. Since 2025.2.
- `options.tools`: tools the model may ask to run, built with
  [`llm.createTool`](#llmcreatetooloptions). Not supported by Cohere Command A Vision. Since 2025.2.
- `options.chatHistory`: earlier turns as [`ChatMessage`](#chatmessage-object) objects. Since
  2024.1.
- `options.preamble`: text that sets context or behaviour for the model. Since 2024.1.

**Returns:** [`llm.Response`](#response-object).

**Errors:**

- `COHERE_VISION_DOES_NOT_SUPPORT_TOOLING`: `options.tools` or `options.toolResults` was passed
  while `options.modelFamily` names a Cohere vision model.
- `DOCUMENT_IDS_MUST_BE_UNIQUE`: two entries in `options.documents` share an ID.
- `DUPLICATE_PARAMETER_NAME`: parameter names repeat (the source ties this to `options` as a
  whole).
- `DUPLICATE_TOOL_NAME`: two tools in `options.tools` share a name.
- `DUPLICATE_TOOL_RESULT`: `options.toolResults` holds the same result twice.
- `INAPPROPRIATE_CONTENT_DETECTED`: the content was rejected under the safety mode. Some filtering
  applies even when the safety-mode restrictions are relaxed.
- `INVALID_FREQUENCY_PENALTY_VALUE`: `options.modelParameters.frequencyPenalty` is not valid.
- `INVALID_IMAGE`: `options.image` is corrupt or not a valid image.
- `INVALID_MAX_TOKENS_VALUE`: `options.modelParameters.maxTokens` is not valid.
- `INVALID_MODEL_FAMILY_VALUE`: `options.modelFamily` is not a valid model family.
- `INVALID_PRESENCE_PENALTY_VALUE`: `options.modelParameters.presencePenalty` is not valid.
- `INVALID_REASONING_EFFORT`: `options.modelParameters.reasoningEffort` is not a
  `ReasoningEffort` value.
- `INVALID_TEMPERATURE_VALUE`: `options.modelParameters.temperature` is not valid for the chosen
  model.
- `INVALID_TOP_K_VALUE`: `options.modelParameters.topK` is not valid.
- `INVALID_TOP_P_VALUE`: `options.modelParameters.topP` is not valid.
- `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`: more than 5 generation requests are running at once.
- `MODEL_1_DOES_NOT_ACCEPT_DOCUMENTS`: `options.documents` was given to a model without RAG
  support.
- `MODEL_1_DOES_NOT_ACCEPT_IMAGE`: `options.image` was given to a model without vision support.
- `MODEL_1_DOES_NOT_ACCEPT_SAFETY_MODE`: `options.safetyMode` was given to a model that does not
  take it.
- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: arguments that conflict, such as `responseFormat` together with
  `documents`, `tools` or `toolResults`, or both penalties on a Cohere Command model.
- `REASONING_EFFORT_PARAMETER_NOT_AVAILABLE`: `reasoningEffort` was given to a model that does not
  support it.
- `RESPONSE_FORMAT_HAS_INVALID_JSON_SCHEMA`: `options.responseFormat` is not a valid JSON schema.
- `SSS_MISSING_REQD_ARGUMENT`: `options.prompt` is missing (and no tool results were given).
- `UNRECOGNIZED_MODEL_PARAMETERS`: `options.modelParameters` contains a key the model does not
  know.
- `UNSUPPORTED_SAFETY_MODE`: `options.safetyMode` is not a `SafetyMode` value.

The `MODEL_1_...` codes are written exactly as the source gives them; the `1` looks like a slot
for a model name, so check the code your account actually returns.

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2024.1

### llm.generateText.promise(options)
<!-- id: N/llm.generateText.promise -->

The asynchronous form of [`llm.generateText`](#llmgeneratetextoptions). It takes the same
parameters, raises the same errors and consumes AI units in the same way, but returns a Promise.
The source lists [`Response`](#response-object) among the objects this variant produces. Its
alternative name is `llm.chat.promise`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.prompt` | string | No | |
| `options.documents` | llm.Document[] | No | |
| `options.image` | file.File | No | |
| `options.modelFamily` | string | No | |
| `options.modelParameters` | Object | No | |
| `options.modelParameters.reasoningEffort` | string | No | |
| `options.modelParameters.topP` | number | No | |
| `options.modelParameters.presencePenalty` | number | No | |
| `options.modelParameters.maxTokens` | number | No | |
| `options.modelParameters.temperature` | number | No | |
| `options.modelParameters.frequencyPenalty` | number | No | |
| `options.modelParameters.topK` | number | No | |
| `options.ociConfig` | Object | No | |
| `options.responseFormat` | Object | No | |
| `options.safetyMode` | string | No | `llm.SafetyMode.STRICT` |
| `options.timeout` | number | No | `30000` |
| `options.toolResults` | llm.ToolResult[] | No | |
| `options.tools` | llm.Tool[] | No | |
| `options.chatHistory` | llm.ChatMessage[] | No | |
| `options.preamble` | string | No | |

**Returns:** `Promise`, settling with an [`llm.Response`](#response-object).

**Errors:** the same as `generateText`.

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2024.1

### llm.generateTextStreamed(options)
<!-- id: N/llm.generateTextStreamed -->

Works like [`llm.generateText`](#llmgeneratetextoptions), but hands back a
[`StreamedResponse`](#streamedresponse-object) that you can use while the model is still writing.
You can read the text produced so far at any moment, or walk the tokens one by one with
`iterator().each(...)` (see [StreamedResponse](#streamedresponse-object)). Every call consumes AI
units. The source also lists the name `llm.chatStreamed`.

> [!SOURCE-ERROR] The sample in the source's description of this function passes the prompt as a
> bare string rather than inside an options object, which contradicts the parameter list. Pass an
> options object with a `prompt` property as documented here.
<!-- source-error: N/llm.generateTextStreamed | - | description_sample_passes_bare_string_prompt_not_options -->

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.prompt` | string | No | |
| `options.documents` | llm.Document[] | No | |
| `options.image` | file.File | No | |
| `options.modelFamily` | enum | No | |
| `options.modelParameters` | Object | No | |
| `options.modelParameters.reasoningEffort` | string | No | |
| `options.modelParameters.topP` | number | No | |
| `options.modelParameters.presencePenalty` | number | No | |
| `options.modelParameters.maxTokens` | number | No | |
| `options.modelParameters.temperature` | number | No | |
| `options.modelParameters.frequencyPenalty` | number | No | |
| `options.modelParameters.topK` | number | No | |
| `options.ociConfig` | Object | No | |
| `options.responseFormat` | Object | No | |
| `options.safetyMode` | string | No | `llm.SafetyMode.STRICT` |
| `options.timeout` | number | No | `30000` |
| `options.toolResults` | llm.ToolResult[] | No | |
| `options.tools` | llm.Tool[] | No | |
| `options.chatHistory` | llm.ChatMessage[] | No | |
| `options.preamble` | string | No | |

The parameters behave as described for `generateText`, with these differences in the spec:

- `options.prompt`, `documents`, `modelFamily`, `modelParameters` (all members except
  `reasoningEffort`), `ociConfig`, `safetyMode`, `timeout`, `chatHistory` and `preamble` are
  available here since 2025.1. `image` is available since 2026.1, `tools` and `toolResults` since
  2025.2, `reasoningEffort` since 2026.2.
- `options.responseFormat` is listed since 2026.1 here, against 2025.1 on `generateText`; the
  spec does not say which date is right.
- `options.modelFamily` is typed `enum` here and `string` on `generateText`. Either way, pass a
  [`llm.ModelFamily`](#llmmodelfamily) value; `COHERE_COMMAND` applies when it is omitted.
- `options.safetyMode` is noted as supported by Cohere models only.
- The spec does not repeat the link to the parameter ranges catalog for this function's
  `modelParameters`.
- Everything else carries over: the prompt is required unless tool results are given (and ignored
  when they are), document IDs must be unique, only Cohere models take documents, only Cohere
  Command A Vision takes an image, Cohere Command A Vision rejects tools and tool results,
  `responseFormat` excludes `documents`, `tools` and `toolResults`, `presencePenalty` and
  `frequencyPenalty` exclude each other on Cohere Command models, `reasoningEffort` defaults to
  `MEDIUM` and works only with OpenAI GPT-OSS, `ociConfig` is always ignored, and the timeout is in
  milliseconds.

**Returns:** [`llm.StreamedResponse`](#streamedresponse-object).

**Errors:** the same codes and conditions as [`generateText`](#llmgeneratetextoptions):

- `COHERE_VISION_DOES_NOT_SUPPORT_TOOLING`: tools or tool results sent to Cohere Command A Vision.
- `DOCUMENT_IDS_MUST_BE_UNIQUE`, `DUPLICATE_PARAMETER_NAME`, `DUPLICATE_TOOL_NAME`,
  `DUPLICATE_TOOL_RESULT`: a document ID, parameter name, tool name or tool result appears twice.
- `INAPPROPRIATE_CONTENT_DETECTED`: content rejected under the safety mode (some filtering applies
  even with relaxed safety mode).
- `INVALID_FREQUENCY_PENALTY_VALUE`, `INVALID_IMAGE`, `INVALID_MAX_TOKENS_VALUE`,
  `INVALID_MODEL_FAMILY_VALUE`, `INVALID_PRESENCE_PENALTY_VALUE`, `INVALID_REASONING_EFFORT`,
  `INVALID_TEMPERATURE_VALUE`, `INVALID_TOP_K_VALUE`, `INVALID_TOP_P_VALUE`: the named argument has
  an invalid value (a corrupt image for `INVALID_IMAGE`; a temperature not valid for the chosen
  model for `INVALID_TEMPERATURE_VALUE`).
- `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`: more than 5 concurrent generation requests.
- `MODEL_1_DOES_NOT_ACCEPT_DOCUMENTS`, `MODEL_1_DOES_NOT_ACCEPT_IMAGE`,
  `MODEL_1_DOES_NOT_ACCEPT_SAFETY_MODE`: documents, an image or a safety mode sent to a model that
  does not take them.
- `MUTUALLY_EXCLUSIVE_ARGUMENTS`: conflicting arguments, as on `generateText`.
- `REASONING_EFFORT_PARAMETER_NOT_AVAILABLE`: `reasoningEffort` sent to a model without support.
- `RESPONSE_FORMAT_HAS_INVALID_JSON_SCHEMA`: `responseFormat` is not a valid JSON schema.
- `SSS_MISSING_REQD_ARGUMENT`: no prompt and no tool results.
- `UNRECOGNIZED_MODEL_PARAMETERS`: an unknown key in `modelParameters`.
- `UNSUPPORTED_SAFETY_MODE`: `safetyMode` is not a `SafetyMode` value.

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2025.1

### llm.generateTextStreamed.promise(options)
<!-- id: N/llm.generateTextStreamed.promise -->

The asynchronous form of [`llm.generateTextStreamed`](#llmgeneratetextstreamedoptions): same
parameters, same errors, consumes AI units, returns a Promise. The source lists
[`StreamedResponse`](#streamedresponse-object) among the objects this variant produces. Its
alternative name is `llm.chatStreamed.promise`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.prompt` | string | No | |
| `options.documents` | llm.Document[] | No | |
| `options.image` | file.File | No | |
| `options.modelFamily` | enum | No | |
| `options.modelParameters` | Object | No | |
| `options.modelParameters.reasoningEffort` | string | No | |
| `options.modelParameters.topP` | number | No | |
| `options.modelParameters.presencePenalty` | number | No | |
| `options.modelParameters.maxTokens` | number | No | |
| `options.modelParameters.temperature` | number | No | |
| `options.modelParameters.frequencyPenalty` | number | No | |
| `options.modelParameters.topK` | number | No | |
| `options.ociConfig` | Object | No | |
| `options.responseFormat` | Object | No | |
| `options.safetyMode` | string | No | `llm.SafetyMode.STRICT` |
| `options.timeout` | number | No | `30000` |
| `options.toolResults` | llm.ToolResult[] | No | |
| `options.tools` | llm.Tool[] | No | |
| `options.chatHistory` | llm.ChatMessage[] | No | |
| `options.preamble` | string | No | |

**Returns:** `Promise`, settling with an [`llm.StreamedResponse`](#streamedresponse-object).

**Errors:** the same as `generateTextStreamed`.

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2025.1

### llm.evaluatePrompt(options)
<!-- id: N/llm.evaluatePrompt -->

Runs a prompt saved in Prompt Studio and returns the model's answer to it. You identify the
prompt by ID and supply values for its template variables. Every call consumes AI units. The
source also lists the name `llm.executePrompt`.

Prompt Studio can write the call for you: its button for showing a SuiteScript example produces a
ready-made `evaluatePrompt` call that already contains the prompt's variables and the values set
in the studio, and you can run that sample in the SuiteScript 2.1 debugger.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string \| number | Yes | |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |
| `options.variables` | Object | No | |

- `options.id`: the prompt's ID, given as a string or a number. A string script ID must begin
  with `custprompt`. Since 2025.1.
- `options.ociConfig`: no longer supported and always ignored. Since 2025.1.
- `options.timeout`: in milliseconds. Since 2025.1.
- `options.variables`: an object whose keys are the prompt's variable names and whose values are
  substituted into the template. Since 2025.1.

**Returns:** [`llm.Response`](#response-object), the model's answer to the saved prompt.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.id` is missing.
- `INVALID_ID_PREFIX`: `options.id` does not begin with `custprompt`.
- `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`: more than 5 LLM requests are running at once.
- `TEMPLATE_PROCESSING_EXCEPTION`: a variable the template needs is missing from
  `options.variables`, or the template contains a syntax error.

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2025.1

### llm.evaluatePrompt.promise(options)
<!-- id: N/llm.evaluatePrompt.promise -->

The asynchronous form of [`llm.evaluatePrompt`](#llmevaluatepromptoptions): same parameters, same
errors, consumes AI units, returns a Promise. Its alternative name is `llm.executePrompt.promise`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string \| number | Yes | |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |
| `options.variables` | Object | No | |

**Returns:** `Promise`. The source does not state what it settles with (the synchronous form
returns a `Response`).

**Errors:** the same as `evaluatePrompt`.

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2025.1

### llm.evaluatePromptStreamed(options)
<!-- id: N/llm.evaluatePromptStreamed -->

Runs a saved Prompt Studio prompt like [`llm.evaluatePrompt`](#llmevaluatepromptoptions), but
returns a [`StreamedResponse`](#streamedresponse-object): you can read partial text before the
model finishes, or step through the tokens with `iterator().each(...)`. Every call consumes AI
units. Prompt Studio's example button generates a call with the variables filled in. The source
also lists the name `llm.executePromptStreamed`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string \| number | Yes | |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |
| `options.variables` | Object | No | |

- `options.id`: a string or a number; a string script ID must begin with `custprompt`. Since
  2025.1.
- `options.ociConfig`: no longer supported and always ignored. Since 2025.1.
- `options.timeout`: in milliseconds. Since 2025.1.
- `options.variables`: an object mapping variable names to the values to substitute. Since 2025.1.

**Returns:** [`llm.StreamedResponse`](#streamedresponse-object), the streamed answer to the saved
prompt.

**Errors:**

- `SSS_MISSING_REQD_ARGUMENT`: `options.id` is missing.
- `INVALID_ID_PREFIX`: `options.id` does not begin with `custprompt`.
- `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`: more than 5 LLM requests are running at once.
- `TEMPLATE_PROCESSING_EXCEPTION`: a template variable is missing from `options.variables`, or the
  template has a syntax error.

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2025.1

### llm.evaluatePromptStreamed.promise(options)
<!-- id: N/llm.evaluatePromptStreamed.promise -->

The asynchronous form of [`llm.evaluatePromptStreamed`](#llmevaluatepromptstreamedoptions): same
parameters, same errors, consumes AI units, returns a Promise. The source lists
[`StreamedResponse`](#streamedresponse-object) among the objects this variant produces. Its
alternative name is `llm.executePromptStreamed.promise`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.id` | string \| number | Yes | |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |
| `options.variables` | Object | No | |

**Returns:** `Promise`, settling with an [`llm.StreamedResponse`](#streamedresponse-object).

**Errors:** the same as `evaluatePromptStreamed`.

> [!SOURCE-ERROR] The source's note on this method's errors refers to the non-streamed
> `evaluatePrompt` instead of `evaluatePromptStreamed`. The spec ties the variant to
> `evaluatePromptStreamed`; the two error lists in the spec are identical anyway.
<!-- source-error: N/llm.evaluatePromptStreamed.promise | - | note_names_non_streamed_evaluateprompt_for_errors -->

**Governance:** 100 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2025.1

### llm.embed(options)
<!-- id: N/llm.embed -->

Converts each input string into an embedding: a vector of numbers that captures the text's
meaning. Comparing embeddings, typically by cosine similarity, supports semantic search, finding
similar items, classifying text and clustering it. A typical case is proposing comparable items
when the one a shopper wants is out of stock. Calls consume NetSuite AI units, and the resulting
vectors must be handled as carefully as the source text. The supported models are the
[`llm.EmbedModelFamily`](#llmembedmodelfamily) values.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.dimensions` | number | No | `1536` |
| `options.embedModelFamily` | string | No | |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |
| `options.truncate` | string | No | |
| `options.inputs` | string[] | Yes | |

- `options.dimensions`: vector length, 1 to 1536. More dimensions capture finer distinctions.
  Regenerate embeddings that were made with older models. Since 2025.2.
- `options.embedModelFamily`: an [`llm.EmbedModelFamily`](#llmembedmodelfamily) value; the
  `cohere.embed-v4.0` model is used when omitted. Since 2025.1.
- `options.ociConfig`: ignored; no longer supported. Since 2025.1.
- `options.timeout`: in milliseconds. Since 2025.1.
- `options.truncate`: an [`llm.Truncate`](#llmtruncate) value deciding what happens to an input
  longer than 512 tokens. Without it, nothing is truncated. Since 2025.1.
- `options.inputs`: the texts to embed, at least 1 and at most 96 per call. Each input may hold up
  to 512 tokens; `truncate` controls what happens beyond that. Since 2025.1.

**Returns:** [`llm.EmbedResponse`](#embedresponse-object).

**Errors:**

- `NO_INPUTS_TO_EMBED`: `options.inputs` is empty.
- `SSS_MISSING_REQD_ARGUMENT`: `options.inputs` is missing.
- `INVALID_MODEL_FAMILY_VALUE`: `options.embedModelFamily` is not an `EmbedModelFamily` value.
- `MAXIMUM_PARALLEL_REQUESTS_LIMIT_EXCEEDED`: more than 5 embed requests are running at once.
- `CAN_EMBED_1_INPUTS_AT_MAXIMUM`: `options.inputs` has more than 96 entries. The code is written
  as the source has it; the `1` looks like a slot for the real number, so verify the code your
  account returns.
- `INVALID_TRUNCATION_METHOD`: `options.truncate` is not a `Truncate` value.
- `UNSUPPORTED_NUMBER_OF_TOKENS`: an input holds too many tokens.

**Governance:** 50 units (AI units are consumed in addition).

**Availability:** server scripts.

**Since:** 2025.1

### llm.embed.promise(options)
<!-- id: N/llm.embed.promise -->

The asynchronous form of [`llm.embed`](#llmembedoptions): same parameters, same errors, returns a
Promise. The source lists [`EmbedResponse`](#embedresponse-object) among the objects this variant
produces.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.dimensions` | number | No | `1536` |
| `options.embedModelFamily` | string | No | |
| `options.ociConfig` | Object | No | |
| `options.timeout` | number | No | `30000` |
| `options.truncate` | string | No | |
| `options.inputs` | string[] | Yes | |

**Returns:** `Promise`, settling with an [`llm.EmbedResponse`](#embedresponse-object).

**Errors:** the same as `embed`.

**Governance:** 50 units.

**Availability:** server scripts.

**Since:** 2025.1

### llm.reRank(options)
<!-- id: N/llm.reRank -->

Scores a list of text documents for relevance to a prompt and returns them ranked, most relevant
first. The result holds only each document's position in your input array and its score, never
the document text or a generated answer, so map the positions back to your own array.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.documents` | string[] | Yes | |
| `options.maxTokensPerDocument` | number | No | `4096` |
| `options.reRankModelFamily` | string | No | |
| `options.timeout` | number | No | `30000` |
| `options.topN` | number | No | `1` |
| `options.prompt` | string | Yes | |

- `options.documents`: 1 to 1000 documents. Since 2026.2.
- `options.maxTokensPerDocument`: 1 to 32768 tokens; longer documents are cut to this length.
  Since 2026.2.
- `options.reRankModelFamily`: an [`llm.ReRankModelFamily`](#llmrerankmodelfamily) value;
  `COHERE_RERANK` when omitted. Since 2026.2.
- `options.timeout`: in milliseconds. Since 2026.2.
- `options.topN`: how many ranked documents to return, from 1 up to the number of documents.
  Since 2026.2.
- `options.prompt`: the query the documents are ranked against. Since 2026.2.

**Returns:** [`llm.ReRankResponse`](#rerankresponse-object), the documents ranked by relevance.

**Errors:**

- `CAN_RERANK_1_DOCUMENTS_AT_MAXIMUM`: `options.documents` has more than 1000 entries.
- `INVALID_MODEL_FAMILY_VALUE`: `options.reRankModelFamily` is not a `ReRankModelFamily` value.
- `MAX_TOKENS_PER_DOCUMENT_CANNOT_EXCEED_1`: `options.maxTokensPerDocument` is above 32768.
- `MAX_TOKENS_PER_DOCUMENT_MUST_BE_POSITIVE`: `options.maxTokensPerDocument` is below 1.
- `MAXIMUM_PARALLEL_RE_RANK_REQUESTS_LIMIT_EXCEEDED`: the parallel re-rank limit has been reached.
  The source does not state the limit's value.
- `NO_DOCUMENTS_TO_RERANK`: `options.documents` is empty.
- `TOP_N_CANNOT_EXCEED_NUMBER_OF_DOCUMENTS`: `options.topN` is larger than the number of
  documents.
- `TOP_N_MUST_BE_POSITIVE`: `options.topN` is below 1.

The two codes containing `_1_` or ending in `_1` are written as the source has them; the `1` looks
like a slot for the real limit (1000 and 32768), so verify the codes your account returns.

**Governance:** 50 units.

**Availability:** server scripts.

**Since:** 2026.2

### llm.reRank.promise(options)
<!-- id: N/llm.reRank.promise -->

The asynchronous form of [`llm.reRank`](#llmrerankoptions): same parameters, same errors, returns a
Promise.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.documents` | string[] | Yes | |
| `options.maxTokensPerDocument` | number | No | `4096` |
| `options.reRankModelFamily` | string | No | |
| `options.timeout` | number | No | `30000` |
| `options.topN` | number | No | `1` |
| `options.prompt` | string | Yes | |

**Returns:** `Promise` that resolves to an [`llm.ReRankResponse`](#rerankresponse-object).

**Errors:** the same as `reRank`.

**Governance:** 50 units.

**Availability:** server scripts.

**Since:** 2026.2

### llm.createChatMessage(options)
<!-- id: N/llm.createChatMessage -->

Builds one turn of a conversation, for the `chatHistory` of
[`generateText`](#llmgeneratetextoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.role` | string | Yes | |
| `options.text` | string | Yes | |

- `options.role`: an [`llm.ChatRole`](#llmchatrole) value. Since 2024.1.
- `options.text`: the message content. Since 2024.1.

**Returns:** [`llm.ChatMessage`](#chatmessage-object), the new chat message.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2024.1

### llm.createDocument(options)
<!-- id: N/llm.createDocument -->

Builds a source document for RAG, to pass in the `documents` of `generateText` or
`generateTextStreamed`. Calling it is optional: a plain object with `id` and `data` properties is
accepted as well.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.data` | string | Yes | |
| `options.id` | string | Yes | |

- `options.data`: the document's content. Since 2025.1.
- `options.id`: the document's identifier; citations refer to it. Since 2025.1.

**Returns:** [`llm.Document`](#document-object), a document ready for RAG.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.1

### llm.createToolParameter(options)
<!-- id: N/llm.createToolParameter -->

Describes one argument of a tool, for use in [`llm.createTool`](#llmcreatetooloptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.type` | string | Yes | |
| `options.name` | string | Yes | |
| `options.description` | string | Yes | |

- `options.type`: an [`llm.ToolParameterType`](#llmtoolparametertype) value. Since 2025.2.
- `options.name`: unique within the tool; tool calls refer to the argument by this name. Since
  2025.2.
- `options.description`: tells the model what the argument means. Since 2025.2.

**Returns:** [`llm.ToolParameter`](#toolparameter-object).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### llm.createTool(options)
<!-- id: N/llm.createTool -->

Defines a tool that `generateText` or `generateTextStreamed` may ask your script to run.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.description` | string | Yes | |
| `options.name` | string | Yes | |
| `options.parameters` | llm.ToolParameter[] | Yes | |

- `options.description`: the model reads this to decide when to request the tool, so state
  clearly what the tool does. Since 2025.2.
- `options.name`: a unique identifier for the tool. Since 2025.2.
- `options.parameters`: the tool's arguments, each created with
  [`llm.createToolParameter`](#llmcreatetoolparameteroptions). Since 2025.2.

**Returns:** [`llm.Tool`](#tool-object).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### llm.createToolResult(options)
<!-- id: N/llm.createToolResult -->

Wraps a tool handler's output so it can go back to the model in the `toolResults` of
`generateText` or `generateTextStreamed`.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.outputs` | Object[] | Yes | |
| `options.call` | llm.ToolCall | Yes | |

- `options.outputs`: the handler's output; each entry typically carries the value in a `result`
  property. Since 2025.2.
- `options.call`: the [`ToolCall`](#toolcall-object) this result answers; it links the result to
  the request that triggered it. Since 2025.2.

**Returns:** [`llm.ToolResult`](#toolresult-object).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.2

### llm.getRemainingUsage()
<!-- id: N/llm.getRemainingUsage -->

Reports how many AI units the account has left.

**Returns:** `number`, the remaining AI units.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2026.1

### llm.getRemainingUsage.promise()
<!-- id: N/llm.getRemainingUsage.promise -->

The asynchronous form of [`llm.getRemainingUsage`](#llmgetremainingusage). Like the synchronous
form it takes no parameters and lists no errors; it returns a Promise.

> [!SOURCE-ERROR] The source's note for this method names `getRemainingFreeUsage` as its
> synchronous counterpart. The spec pairs it with `getRemainingUsage`; use that when you want the
> synchronous call.
<!-- source-error: N/llm.getRemainingUsage.promise | - | note_names_getremainingfreeusage_as_sync_version -->

**Returns:** `Promise`. The source does not state what it settles with (the synchronous form
returns a number).

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2026.1

### llm.getRemainingFreeUsage()
<!-- id: N/llm.getRemainingFreeUsage -->

**Deprecated.** Returns the remaining AI units by delegating to
[`llm.getRemainingUsage`](#llmgetremainingusage); call that in new code.

**Returns:** `number`, the remaining AI units.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2024.1 (deprecated).

### llm.getRemainingFreeUsage.promise()
<!-- id: N/llm.getRemainingFreeUsage.promise -->

**Deprecated.** The asynchronous form of `getRemainingFreeUsage`. It delegates to
[`llm.getRemainingUsage.promise`](#llmgetremainingusagepromise) and reports the remaining AI
units.

**Returns:** `Promise` for the remaining AI units.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2024.1 (deprecated).

### llm.getRemainingFreeEmbedUsage()
<!-- id: N/llm.getRemainingFreeEmbedUsage -->

**Deprecated.** Returns the remaining AI units by delegating to
[`llm.getRemainingUsage`](#llmgetremainingusage), so it reports the same pool.

**Returns:** `number`, the remaining AI units.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.1 (deprecated).

### llm.getRemainingFreeEmbedUsage.promise()
<!-- id: N/llm.getRemainingFreeEmbedUsage.promise -->

**Deprecated.** The asynchronous form of `getRemainingFreeEmbedUsage`. It delegates to
[`llm.getRemainingUsage.promise`](#llmgetremainingusagepromise).

**Returns:** `Promise` for the remaining AI units.

**Governance:** 0 units.

**Availability:** server scripts.

**Since:** 2025.1 (deprecated).

## Objects

All objects are server-side. Their read-only properties throw `READ_ONLY` when assigned.

### Response (object)
<!-- id: N/llm.Response -->

The result of [`generateText`](#llmgeneratetextoptions),
[`generateText.promise`](#llmgeneratetextpromiseoptions) and
[`evaluatePrompt`](#llmevaluatepromptoptions). Since 2024.1.

#### Response.text
<!-- id: N/llm.Response#text -->

`string`, read-only. The text of the model's answer. Assigning it throws `READ_ONLY`. Since 2024.1.

#### Response.model
<!-- id: N/llm.Response#model -->

`string`, read-only. The ID of the model that wrote the answer. Assigning it throws `READ_ONLY`.
Since 2024.1.

#### Response.usage
<!-- id: N/llm.Response#usage -->

`llm.Usage`, read-only. Token counts for the request as a [`Usage`](#usage-object) object,
provided for Cohere models. Assigning it throws `READ_ONLY`. Since 2025.1.

#### Response.citations
<!-- id: N/llm.Response#citations -->

`llm.Citation[]`, read-only. [`Citation`](#citation-object) entries linking parts of the answer to
source documents; present when supplied documents were used. Assigning it throws `READ_ONLY`.
Since 2025.1.

#### Response.documents
<!-- id: N/llm.Response#documents -->

`llm.Document[]`, read-only. The [`Document`](#document-object) objects the model drew on; present
when supplied documents were used. Use it to confirm which content the answer relied on.
Assigning it throws `READ_ONLY`. Since 2025.1.

#### Response.toolCalls
<!-- id: N/llm.Response#toolCalls -->

`llm.ToolCall[]`, read-only. The tool runs the model asks your script to perform, as
[`ToolCall`](#toolcall-object) objects. When it comes back empty, the model is done with tools.
Assigning it throws `READ_ONLY`. Since 2025.2.

#### Response.chatHistory
<!-- id: N/llm.Response#chatHistory -->

`llm.ChatMessage[]`, read-only. The conversation so far as [`ChatMessage`](#chatmessage-object)
objects, ready to pass into the next call's `chatHistory`. Assigning it throws `READ_ONLY`. Since
2024.1.

### StreamedResponse (object)
<!-- id: N/llm.StreamedResponse -->

The result of [`generateTextStreamed`](#llmgeneratetextstreamedoptions),
[`evaluatePromptStreamed`](#llmevaluatepromptstreamedoptions) and their promise forms. Since
2025.1.

- While generation is still running you can read `text` (the response up to the current token)
  and `model`.
- `documents`, `citations`, `chatHistory` and `toolCalls` are filled in only once the full
  response exists.
- **Reading token by token:** call `iterator()` on the streamed response, then `each` on the
  iterator it returns: `streamed.iterator().each(callback)`. The callback runs once per token and
  receives a token whose `value` property holds that token's text. The iterator is the global
  [Iterator](../globals.md#iteratorcallback): return `true` from the callback to keep reading and
  `false` to stop. During iteration, `text` holds the response up to the current token. The spec
  names `iterator()` only as the way to iterate the stream; it gives no separate signature or
  section for it.

#### StreamedResponse.text
<!-- id: N/llm.StreamedResponse#text -->

`string`, read-only. The answer text: partial while streaming, complete at the end. Assigning it
throws `READ_ONLY`. Since 2025.1.

#### StreamedResponse.model
<!-- id: N/llm.StreamedResponse#model -->

`string`, read-only. The model ID, available from the moment the stream starts. Assigning it
throws `READ_ONLY`. Since 2025.1.

#### StreamedResponse.citations
<!-- id: N/llm.StreamedResponse#citations -->

`llm.Citation[]`, read-only. Citations into the supplied documents, available only after the full
response is generated and only when documents were used. Assigning it throws `READ_ONLY`. Since
2025.1.

#### StreamedResponse.documents
<!-- id: N/llm.StreamedResponse#documents -->

`llm.Document[]`, read-only. The documents used, available only after the full response is
generated and only when supplied documents were used. Assigning it throws `READ_ONLY`. Since
2025.1.

#### StreamedResponse.chatHistory
<!-- id: N/llm.StreamedResponse#chatHistory -->

`llm.ChatMessage[]`, read-only. The conversation history, available after the full response is
generated. Assigning it throws `READ_ONLY`. Since 2025.1.

#### StreamedResponse.toolCalls
<!-- id: N/llm.StreamedResponse#toolCalls -->

`llm.ToolCall[]`, read-only. Tool requests from the model, complete only after the full response
is generated; read the stream to the end before running any tools. Assigning it throws
`READ_ONLY`. Since 2025.2.

> [!SOURCE-ERROR] The source's member table gives this property's parent object as `llm.Response`
> rather than `llm.StreamedResponse`. The spec places it on `StreamedResponse` (`Response` has its
> own `toolCalls`), so read it from the streamed response.
<!-- source-error: N/llm.StreamedResponse#toolCalls | - | parent_object_row_says_llm_response -->

### Usage (object)
<!-- id: N/llm.Usage -->

Token counts for a request, found in [`Response.usage`](#responseusage) when a Cohere model was
used. It holds `completionTokens`, `promptTokens` and `totalTokens`. Server scripts; since 2025.1.

#### Usage.completionTokens
<!-- id: N/llm.Usage#completionTokens -->

`number`, read-only. Tokens in the model's answer. Assigning it throws `READ_ONLY`. Since 2025.1.

#### Usage.promptTokens
<!-- id: N/llm.Usage#promptTokens -->

`number`, read-only. Input tokens, counting the prompt, the preamble and any documents. Assigning
it throws `READ_ONLY`. Since 2025.1.

#### Usage.totalTokens
<!-- id: N/llm.Usage#totalTokens -->

`number`, read-only. `completionTokens` plus `promptTokens`. Assigning it throws `READ_ONLY`. Since
2025.1.

### ChatMessage (object)
<!-- id: N/llm.ChatMessage -->

One turn in a conversation, returned by [`llm.createChatMessage`](#llmcreatechatmessageoptions).
Server scripts; since 2024.1.

#### ChatMessage.text
<!-- id: N/llm.ChatMessage#text -->

`string`. The message content. The source does not state whether this property is read-only.
Since 2024.1.

#### ChatMessage.role
<!-- id: N/llm.ChatMessage#role -->

`string`, read-only. Who wrote the message, as an [`llm.ChatRole`](#llmchatrole) value. Assigning
it throws `READ_ONLY`. Since 2024.1.

### Document (object)
<!-- id: N/llm.Document -->

A piece of source content for RAG, returned by [`llm.createDocument`](#llmcreatedocumentoptions)
and handed back in [`Response.documents`](#responsedocuments) when it was used. Server scripts;
since 2025.1.

- The model weighs supplied documents against what it already holds to be true. If two documents
  contradict each other on the same subject, the answer may side with the one that fits
  established knowledge.
- There is no size limit per document, but everything sent must fit the model's context window,
  which is 256,000 tokens for Cohere Command A. Content beyond the window can cause an error.
- Check `Response.documents` to confirm which content the answer actually used.

#### Document.data
<!-- id: N/llm.Document#data -->

`string`, read-only. The document's content, set through `options.data` of `createDocument`.
Assigning it throws `READ_ONLY`. Since 2025.1.

> [!SOURCE-ERROR] The source lists `llm.Citation` as this property's parent object, but it belongs
> to `llm.Document`. Read it from document objects.
<!-- source-error: N/llm.Document#data | - | parent_object_row_says_llm_citation -->

#### Document.id
<!-- id: N/llm.Document#id -->

`string`, read-only. The document's identifier, set through `options.id` of `createDocument`;
citations point to documents by this value. Assigning it throws `READ_ONLY`. Since 2025.1.

> [!SOURCE-ERROR] As with `data`, the source names `llm.Citation` as the parent object, but the
> property belongs to `llm.Document`.
<!-- source-error: N/llm.Document#id | - | parent_object_row_says_llm_citation -->

### Citation (object)
<!-- id: N/llm.Citation -->

Produced by RAG when documents are given to `generateText` (or its streamed form). Each citation
marks a stretch of the answer and names the documents it came from. Found in
[`Response.citations`](#responsecitations). Server scripts; since 2025.1.

#### Citation.text
<!-- id: N/llm.Citation#text -->

`string`, read-only. The cited passage. Assigning it throws `READ_ONLY`. Since 2025.1.

#### Citation.start
<!-- id: N/llm.Citation#start -->

`number`, read-only. The character offset in the response where the cited passage begins.
Assigning it throws `READ_ONLY`. Since 2025.1.

> [!SOURCE-ERROR] The source first calls this a position in the document and then a position in
> the response. The spec reads it as an offset into the response text, matching `end`; check
> against a real response before relying on either reading.
<!-- source-error: N/llm.Citation#start | - | says_position_in_document_then_response -->

#### Citation.end
<!-- id: N/llm.Citation#end -->

`number`, read-only. The character offset in the response where the cited passage ends; a
citation might, for instance, cover characters 52 through 83 of the answer. Assigning it throws
`READ_ONLY`. Since 2025.1.

#### Citation.documentIds
<!-- id: N/llm.Citation#documentIds -->

`string[]`. The [`Document.id`](#documentid) values of the source documents; when the cited
information appears in several documents, several IDs are listed. The source does not state
whether this property is read-only. Since 2025.1.

### Tool (object)
<!-- id: N/llm.Tool -->

A tool definition holding a name, a description and parameters, returned by
[`llm.createTool`](#llmcreatetooloptions). Server scripts; since 2025.2.

#### Tool.name
<!-- id: N/llm.Tool#name -->

`string`, read-only. The tool's name. Assigning it throws `READ_ONLY`. Since 2025.2.

#### Tool.description
<!-- id: N/llm.Tool#description -->

`string`, read-only. The tool's description. Assigning it throws `READ_ONLY`. Since 2025.2.

#### Tool.parameters
<!-- id: N/llm.Tool#parameters -->

`llm.ToolParameter[]`, read-only. The tool's arguments as [`ToolParameter`](#toolparameter-object)
objects. Assigning it throws `READ_ONLY`. Since 2025.2.

### ToolParameter (object)
<!-- id: N/llm.ToolParameter -->

One argument of a tool, holding a name, a description and a type. Returned by
[`llm.createToolParameter`](#llmcreatetoolparameteroptions). Server scripts; since 2025.2.

#### ToolParameter.name
<!-- id: N/llm.ToolParameter#name -->

`string`, read-only. The argument's name. Assigning it throws `READ_ONLY`. Since 2025.2.

#### ToolParameter.type
<!-- id: N/llm.ToolParameter#type -->

`string`, read-only. An [`llm.ToolParameterType`](#llmtoolparametertype) value. Assigning it
throws `READ_ONLY`. Since 2025.2.

#### ToolParameter.description
<!-- id: N/llm.ToolParameter#description -->

`string`, read-only. The argument's description. Assigning it throws `READ_ONLY`. Since 2025.2.

### ToolCall (object)
<!-- id: N/llm.ToolCall -->

The model's request for your script to run a tool, found in
[`Response.toolCalls`](#responsetoolcalls) and
[`StreamedResponse.toolCalls`](#streamedresponsetoolcalls). It carries the tool's name and the
arguments the model chose. Your script runs the matching handler and sends the outcome back with
[`llm.createToolResult`](#llmcreatetoolresultoptions). Server scripts; since 2025.2.

#### ToolCall.name
<!-- id: N/llm.ToolCall#name -->

`string`, read-only. The name of the tool to run. Assigning it throws `READ_ONLY`. Since 2025.2.

#### ToolCall.parameters
<!-- id: N/llm.ToolCall#parameters -->

`llm.ToolParameter[]`, read-only. The arguments the model supplies for this call. Each entry is
typed as a [`ToolParameter`](#toolparameter-object), which exposes the read-only properties
`name`, `type` (an [`llm.ToolParameterType`](#llmtoolparametertype) value) and `description`. The
source does not state which property, if any, holds the value the model chose for the argument;
inspect a real call (for example with `JSON.stringify`) before writing a handler that reads
argument values. Assigning it throws `READ_ONLY`. Since 2025.2.

### ToolResult (object)
<!-- id: N/llm.ToolResult -->

A tool's output together with the call it answers, returned by
[`llm.createToolResult`](#llmcreatetoolresultoptions). Server scripts; since 2025.2.

#### ToolResult.outputs
<!-- id: N/llm.ToolResult#outputs -->

`Object[]`, read-only. The tool's outputs. Assigning it throws `READ_ONLY`. Since 2025.2.

#### ToolResult.call
<!-- id: N/llm.ToolResult#call -->

`llm.ToolCall`, read-only. The [`ToolCall`](#toolcall-object) that led to this result. Assigning it
throws `READ_ONLY`. Since 2025.2.

### EmbedResponse (object)
<!-- id: N/llm.EmbedResponse -->

The result of [`embed`](#llmembedoptions) and [`embed.promise`](#llmembedpromiseoptions). Its
contents are as sensitive as the source data. Server scripts; since 2025.1.

#### EmbedResponse.inputs
<!-- id: N/llm.EmbedResponse#inputs -->

`string[]`, read-only. The texts that were embedded. Assigning it throws `READ_ONLY`. Since 2025.1.

#### EmbedResponse.model
<!-- id: N/llm.EmbedResponse#model -->

`string`, read-only. The ID of the embedding model used. Assigning it throws `READ_ONLY`. Since
2025.1.

#### EmbedResponse.embeddings
<!-- id: N/llm.EmbedResponse#embeddings -->

`number[][]`, read-only. One vector per input. Guard the vectors as closely as the text they were
built from. Assigning it throws `READ_ONLY`. Since 2025.1.

### ReRankResponse (object)
<!-- id: N/llm.ReRankResponse -->

The result of [`reRank`](#llmrerankoptions) and [`reRank.promise`](#llmrerankpromiseoptions),
holding `documentRanks` and `model`. Server scripts; since 2026.2.

#### ReRankResponse.model
<!-- id: N/llm.ReRankResponse#model -->

`string`, read-only. The ID of the re-rank model used. Assigning it throws `READ_ONLY`. Since
2026.2.

#### ReRankResponse.documentRanks
<!-- id: N/llm.ReRankResponse#documentRanks -->

`llm.DocumentRank[]`, read-only. [`DocumentRank`](#documentrank-object) entries ordered from most
to least relevant. `options.topN` of `reRank` sets how many there are. Document text is not
included. Assigning it throws `READ_ONLY`. Since 2026.2.

### DocumentRank (object)
<!-- id: N/llm.DocumentRank -->

One ranked document, with `index` and `relevance` but without the document text. Found in
[`ReRankResponse.documentRanks`](#rerankresponsedocumentranks). Server scripts; since 2026.2.

#### DocumentRank.relevance
<!-- id: N/llm.DocumentRank#relevance -->

`number`, read-only. The relevance score; the list is ordered most relevant first. Assigning it
throws `READ_ONLY`. Since 2026.2.

#### DocumentRank.index
<!-- id: N/llm.DocumentRank#index -->

`number`, read-only. The document's position in the `documents` array you passed. It is **not**
the rank; the rank is the entry's position within `documentRanks`. Assigning it throws
`READ_ONLY`. Since 2026.2.

## Enums

### llm.ModelFamily
<!-- id: N/llm.ModelFamily -->

Text-generation models for `options.modelFamily` of [`generateText`](#llmgeneratetextoptions) and
[`generateTextStreamed`](#llmgeneratetextstreamedoptions). `COHERE_COMMAND` applies when none is
given. Since 2024.2.

| Value | Model ID | RAG (documents) | Preamble | Image input |
|---|---|---|---|---|
| `COHERE_COMMAND` | `cohere.command-a-03-2025` | Yes | Yes | No |
| `COHERE_COMMAND_LATEST` | `cohere.command-a-03-2025` | Yes | Yes | No |
| `COHERE_COMMAND_VISION` | `cohere.command-a-vision` | No | Yes | Yes |
| `COHERE_COMMAND_VISION_LATEST` | `cohere.command-a-vision` | No | Yes | Yes |
| `GPT_OSS` | `openai.gpt-oss-120b` | No | Yes | No |
| `GPT_OSS_LATEST` | `openai.gpt-oss-120b` | No | Yes | No |

### llm.EmbedModelFamily
<!-- id: N/llm.EmbedModelFamily -->

Embedding models for `options.embedModelFamily` of [`embed`](#llmembedoptions). The embeddings
they produce are as sensitive as the source data. Since 2025.1.

| Value | Model ID |
|---|---|
| `COHERE_EMBED` | `cohere.embed-v4.0` |
| `COHERE_EMBED_LATEST` | `cohere.embed-v4.0` |

### llm.ReRankModelFamily
<!-- id: N/llm.ReRankModelFamily -->

Re-rank models for `options.reRankModelFamily` of [`reRank`](#llmrerankoptions). `COHERE_RERANK`
applies when none is given. Since 2026.2.

| Value | Model ID |
|---|---|
| `COHERE_RERANK` | `cohere.rerank-v4.0-fast` |
| `COHERE_RERANK_LATEST` | `cohere.rerank-v4.0-fast` |

### llm.SafetyMode
<!-- id: N/llm.SafetyMode -->

How strictly a Cohere model avoids sensitive content; it applies to Cohere models. Used by
`options.safetyMode` of the generation functions, with `STRICT` when none is given. Since 2025.1.

| Value | Meaning |
|---|---|
| `CONTEXTUAL` | Looser, for creative or academic work |
| `STRICT` | Avoids sensitive topics; meant for business use |

> [!SOURCE-ERROR] The source's error descriptions mention an "off" safety mode, but the enum holds
> only `CONTEXTUAL` and `STRICT`. Use one of those two; any other value fails with
> `UNSUPPORTED_SAFETY_MODE`.
<!-- source-error: N/llm.SafetyMode | - | errors_mention_off_mode_not_listed -->

### llm.ReasoningEffort
<!-- id: N/llm.ReasoningEffort -->

How much reasoning the OpenAI GPT-OSS model does, for `options.modelParameters.reasoningEffort`.
Only that model supports it, and on it `MEDIUM` applies when none is given. Higher effort uses
more tokens, more AI units and more time, so pick the lowest level that still meets your quality
bar. Since 2026.2.

| Value | Meaning |
|---|---|
| `LOW` | Fastest, fewest tokens |
| `MEDIUM` | Balanced; general purpose |
| `HIGH` | Most thorough, for complex tasks |

### llm.ChatRole
<!-- id: N/llm.ChatRole -->

Author of a chat message, for `options.role` of
[`createChatMessage`](#llmcreatechatmessageoptions). Since 2024.1.

| Value | Meaning |
|---|---|
| `USER` | The side that sent the prompt |
| `CHATBOT` | The side that sent back the response |

### llm.Truncate
<!-- id: N/llm.Truncate -->

What [`embed`](#llmembedoptions) does with an input longer than 512 tokens. Since 2025.1.

| Value | Meaning |
|---|---|
| `END` | Cut tokens from the end |
| `NONE` | Do not truncate |
| `START` | Cut tokens from the start |

### llm.ToolParameterType
<!-- id: N/llm.ToolParameterType -->

Data type of a tool argument, for `options.type` of
[`createToolParameter`](#llmcreatetoolparameteroptions). Since 2025.2.

| Value |
|---|
| `ARRAY` |
| `BOOLEAN` |
| `FLOAT` |
| `INTEGER` |
| `OBJECT` |
| `STRING` |

## Examples

A user event script that drafts an internal triage note for each new support case by running a
saved Prompt Studio prompt. It checks the AI unit balance first, so an exhausted allowance is
logged without affecting the case save. The prompt script ID is invented.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType UserEventScript
 */
define(['N/llm', 'N/log'], (llm, log) => {
  const afterSubmit = (context) => {
    if (context.type !== context.UserEventType.CREATE) {
      return;
    }
    if (llm.getRemainingUsage() <= 0) {
      log.audit({ title: 'Case triage skipped', details: 'No AI units left' });
      return;
    }
    const caseRec = context.newRecord;
    try {
      const answer = llm.evaluatePrompt({
        id: 'custprompt_case_triage_note',
        variables: {
          subject: caseRec.getValue({ fieldId: 'title' }),
          message: caseRec.getValue({ fieldId: 'incomingmessage' })
        },
        timeout: 20000
      });
      log.audit({ title: `Triage note for case ${caseRec.id}`, details: answer.text });
    } catch (e) {
      log.error({ title: 'Triage prompt failed', details: `${e.name}: ${e.message}` });
    }
  };

  return { afterSubmit };
});
```

A RESTlet that answers a returns-policy question only from the supplied policy texts (RAG) and
reports which documents each cited passage came from.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/llm'], (llm) => {
  const policies = [
    { id: 'returns-eu', text: 'EU customers may return unopened goods within 30 days.' },
    { id: 'returns-us', text: 'US customers may return goods within 14 days with a receipt.' },
    { id: 'restock', text: 'Opened electronics carry a 10 percent restocking fee.' }
  ];

  const post = (body) => {
    const documents = policies.map((p) => llm.createDocument({ id: p.id, data: p.text }));
    const response = llm.generateText({
      prompt: body.question,
      preamble: 'Answer only from the supplied policy documents. Say so if they do not cover it.',
      documents,
      modelFamily: llm.ModelFamily.COHERE_COMMAND,
      modelParameters: { temperature: 0.1, maxTokens: 250 }
    });
    const sources = (response.citations || []).map((c) => ({
      passage: c.text,
      from: c.documentIds
    }));
    return { answer: response.text, sources };
  };

  return { post };
});
```

Streaming a freight-risk summary in a Suitelet. The script walks the tokens with
`iterator().each`, collects each token's `value`, logs progress every 25 tokens using the partial
`text`, and stops early once a length budget is spent by returning `false` from the callback.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/llm', 'N/log'], (llm, log) => {
  const CHAR_BUDGET = 1200;

  const onRequest = (context) => {
    const cargo = context.request.parameters.cargo || 'boxed ceramic tiles';
    const streamed = llm.generateTextStreamed({
      prompt: `List the main damage risks when shipping ${cargo} by sea, one line each.`,
      modelParameters: { maxTokens: 300, temperature: 0.2 }
    });

    const pieces = [];
    let length = 0;
    streamed.iterator().each((token) => {
      pieces.push(token.value);
      length += token.value.length;
      if (pieces.length % 25 === 0) {
        log.debug({ title: `Progress after ${pieces.length} tokens`, details: streamed.text });
      }
      return length < CHAR_BUDGET;
    });

    context.response.write({
      output: `${pieces.join('')}\n\n(${pieces.length} tokens from ${streamed.model})`
    });
  };

  return { onRequest };
});
```

A Suitelet for administrators that lets the model call one tool, which reports the account's AI
unit balance, and loops until no further tool calls come back. The tool's single argument is only
a note on why the balance is wanted; the handler logs the raw `parameters` list instead of
reading a value from it, because the source does not say where the chosen value sits.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Suitelet
 */
define(['N/llm', 'N/log'], (llm, log) => {
  const balanceTool = llm.createTool({
    name: 'ai_unit_balance',
    description: 'Returns how many NetSuite AI units the account has left today.',
    parameters: [
      llm.createToolParameter({
        name: 'purpose',
        type: llm.ToolParameterType.STRING,
        description: 'A short note on why the balance is needed'
      })
    ]
  });

  const runTool = (call) => {
    log.debug({ title: `Tool call ${call.name}`, details: JSON.stringify(call.parameters) });
    if (call.name !== 'ai_unit_balance') {
      return -1; // flag value: unknown tool
    }
    return llm.getRemainingUsage();
  };

  const onRequest = (context) => {
    let response = llm.generateText({
      prompt: context.request.parameters.question || 'Can we afford tonight\'s batch of 40 summaries?',
      tools: [balanceTool]
    });

    let rounds = 0;
    while (response.toolCalls && response.toolCalls.length > 0 && rounds < 4) {
      const toolResults = response.toolCalls.map((call) =>
        llm.createToolResult({ call, outputs: [{ result: runTool(call) }] })
      );
      response = llm.generateText({
        tools: [balanceTool],
        toolResults,
        chatHistory: response.chatHistory
      });
      rounds += 1;
    }

    context.response.write({ output: response.text });
  };

  return { onRequest };
});
```

Proposing substitutes for an out-of-stock item: embed the item descriptions, shortlist them by
cosine similarity, then let `reRank` order the shortlist against the shopper's own wording.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/llm'], (llm) => {
  const cosine = (a, b) => {
    let dot = 0;
    let na = 0;
    let nb = 0;
    for (let i = 0; i < a.length; i += 1) {
      dot += a[i] * b[i];
      na += a[i] * a[i];
      nb += b[i] * b[i];
    }
    return dot / (Math.sqrt(na) * Math.sqrt(nb));
  };

  const suggestSubstitutes = (wantedDescription, candidateDescriptions, shopperQuery) => {
    const emb = llm.embed({
      inputs: [wantedDescription, ...candidateDescriptions],
      embedModelFamily: llm.EmbedModelFamily.COHERE_EMBED,
      truncate: llm.Truncate.END
    });
    const [target, ...others] = emb.embeddings;
    const shortlist = others
      .map((vec, i) => ({ text: candidateDescriptions[i], score: cosine(target, vec) }))
      .sort((x, y) => y.score - x.score)
      .slice(0, 10)
      .map((entry) => entry.text);

    const ranked = llm.reRank({
      prompt: shopperQuery,
      documents: shortlist,
      topN: Math.min(3, shortlist.length)
    });
    return ranked.documentRanks.map((r) => ({ description: shortlist[r.index], relevance: r.relevance }));
  };

  return { suggestSubstitutes };
});
```
