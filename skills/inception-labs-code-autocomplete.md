---
name: Inline code autocomplete (FIM)
description: Generate IDE-style inline code completions with the Inception fill-in-the-middle endpoint, powered by Mercury Edit 2.
api: openapi/inception-labs-openapi-original.json
operations: [listFIMModels, createFimCompletion]
---

# Fill-in-the-middle autocomplete

Use this for low-latency inline code completion given the code before (and optionally after) the cursor.

## Steps
1. (Optional) `listFIMModels` — `GET /v1/fim/completions/models` to confirm a FIM model id (e.g. `mercury-edit-2`).
2. `createFimCompletion` — `POST /v1/fim/completions` with:
   - `model`: `mercury-edit-2`
   - `prompt`: the code prefix (before the cursor)
   - `suffix` (optional): the code after the cursor
   - `stream` (optional): `true` for `TextCompletionChunk` deltas.
3. Insert the returned completion text at the cursor.

## Rules
- FIM does NOT support tool calling or `response_format` — it is completion-only.
- Designed for latency-sensitive editor loops; prefer non-streaming for short single-line completions, streaming for multi-line.
- Same error envelope and auth as chat (see errors/ and authentication/).
