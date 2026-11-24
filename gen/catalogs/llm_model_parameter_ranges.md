# Catalog: llm_model_parameter_ranges
<!-- id: catalog:llm_model_parameter_ranges -->

Used with: `N/llm.generateText`, `N/llm.generateTextStreamed`

| ID | Label | Value | Tags |
|---|---|---|---|
| `command_a_max_tokens` | maxTokens | 1-4000; default 2000 | cohere_command_a, cohere_command_a_vision |
| `gpt_oss_max_tokens` | maxTokens | 1-16000; default 2000 | openai_gpt_oss |
| `command_a_frequency_penalty` | frequencyPenalty | 0-1; default 0 | cohere_command_a, cohere_command_a_vision |
| `gpt_oss_frequency_penalty` | frequencyPenalty | -2 to 2; default 0 | openai_gpt_oss |
| `command_a_presence_penalty` | presencePenalty | 0-1; default 0 | cohere_command_a, cohere_command_a_vision |
| `gpt_oss_presence_penalty` | presencePenalty | -2 to 2; default 0 | openai_gpt_oss |
| `command_a_prompt_tokens` | prompt | input limit 256000 tokens | cohere_command_a, cohere_command_a_vision |
| `gpt_oss_prompt_tokens` | prompt | input limit 128000 tokens | openai_gpt_oss |
| `command_a_temperature` | temperature | 0-1; default 0.2 | cohere_command_a, cohere_command_a_vision |
| `gpt_oss_temperature` | temperature | 0-2; default 0.2 | openai_gpt_oss |
| `command_a_top_k` | topK | 0-500; default 500 | cohere_command_a, cohere_command_a_vision |
| `gpt_oss_top_k` | topK | 0-100000; default 500 | openai_gpt_oss |
| `all_top_p` | topP | 0-1; default 0.7 | cohere_command_a, cohere_command_a_vision, openai_gpt_oss |
