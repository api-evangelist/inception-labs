---
name: Get structured JSON output from Mercury 2
description: Constrain a Mercury 2 chat completion to a JSON schema so the response is strictly typed and machine-parseable.
api: openapi/inception-labs-openapi-original.json
operations: [createChatCompletion]
---

# Structured JSON output

Use this when you need a Mercury 2 response that conforms exactly to a JSON schema.

## Steps
1. `createChatCompletion` — `POST /v1/chat/completions` with:
   - `model`: `mercury-2`
   - `messages`: your prompt
   - `response_format`: `{"type":"json_schema","json_schema":{"name":"...","schema":{...},"strict":true}}` (ResponseFormatJSONSchema in the spec). `{"type":"json_object"}` is also supported for loose JSON.
2. Parse `choices[0].message.content` as JSON — with `json_schema` it is guaranteed to match the schema.

## Rules
- Only the chat endpoint supports `response_format`; FIM and Edit do not.
- Keep the schema within the model's 128K context; oversized schemas + input trigger `400 context_length_exceeded`.
- See conventions/inception-labs-conventions.yml (structured_output) and errors/inception-labs-problem-types.yml.
