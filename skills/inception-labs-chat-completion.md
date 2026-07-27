---
name: Chat with Mercury 2
description: Send a chat completion to Mercury 2 over the OpenAI-compatible Inception API, with optional reasoning effort and streaming.
api: openapi/inception-labs-openapi-original.json
operations: [listChatModels, createChatCompletion]
---

# Chat with Mercury 2

Use this to generate a conversational response from Inception's Mercury 2 diffusion LLM.

## Auth
All requests need `Authorization: Bearer <api_key>`. Get a key at https://platform.inceptionlabs.ai. Base URL is `https://api.inceptionlabs.ai/v1`.

## Steps
1. (Optional) `listChatModels` — `GET /v1/chat/completions/models` to confirm a chat model id (e.g. `mercury-2`).
2. `createChatCompletion` — `POST /v1/chat/completions` with:
   - `model`: `mercury-2`
   - `messages`: `[{"role":"user","content":"..."}]`
   - `reasoning_effort` (optional): use `instant` for near-realtime, low-latency turns (voice); omit for full reasoning.
   - `stream` (optional): `true` to receive `text/event-stream` `ChatCompletionChunk` deltas terminated by `data: [DONE]`.
3. Read `choices[0].message.content` (or accumulate streamed deltas).

## Rules
- The API is OpenAI-compatible — any OpenAI client works by pointing `base_url` at the URL above.
- No idempotency key exists; do not assume safe retries return the same completion (see conventions/inception-labs-conventions.yml).
- Handle errors from the `{"error":{message,type,param,code}}` envelope: `401` bad key, `402` billing/quota, `429` rate limit (back off), `400` `context_length_exceeded` (trim input). See errors/inception-labs-problem-types.yml.
