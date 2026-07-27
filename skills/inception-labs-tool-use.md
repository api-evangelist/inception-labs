---
name: Tool calling with Mercury 2
description: Let Mercury 2 invoke functions/tools on the chat completions endpoint to orchestrate multi-step agent workflows.
api: openapi/inception-labs-openapi-original.json
operations: [createChatCompletion]
---

# Tool calling

Use this to give Mercury 2 access to functions it can call during a chat completion.

## Steps
1. `createChatCompletion` — `POST /v1/chat/completions` with:
   - `model`: `mercury-2`
   - `messages`: the conversation so far
   - `tools`: `[{"type":"function","function":{"name":"...","description":"...","parameters":{...}}}]` (ChatCompletionTool / FunctionDefinition in the spec).
2. If the model wants a tool, `choices[0].message.tool_calls[]` contains `function.name` + `function.arguments` (JSON string). Execute the function yourself.
3. Append a `{"role":"tool","tool_call_id":...,"content":...}` message and call `createChatCompletion` again to let the model continue.
4. Repeat until the model returns a final assistant message with no tool calls.

## Rules
- Do NOT set `reasoning_effort: instant` when you need tool calling — the instant path skips tools (see conventions).
- Tool calling is chat-only; FIM/Edit endpoints ignore `tools`.
- Back off on `429`; surface `402` (billing) to the operator.
