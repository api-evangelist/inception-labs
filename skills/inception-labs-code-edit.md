---
name: Apply a code edit (Next Edit)
description: Generate an edit to a specific code region given the surrounding file context, using the Inception edit completions endpoint.
api: openapi/inception-labs-openapi-original.json
operations: [listEditModels, createEditCompletion]
---

# Code edit completion

Use this to have Mercury Edit 2 rewrite a marked region of code in place given the whole-file context.

## Steps
1. (Optional) `listEditModels` — `GET /v1/edit/completions/models` for the edit model id (e.g. `mercury-edit-2`).
2. `createEditCompletion` — `POST /v1/edit/completions` with a single user message whose content includes the required edit prompt tags:
   - `<|current_file_content|>` … the full file
   - `<|code_to_edit|>` … the region to change
   - `<|cursor|>` … the cursor position
3. Apply the returned `EditCompletionResponse` edit to the region.

## Rules
- The request must contain exactly one user message and include the required tag markers, or you get `400 invalid_request_error`.
- Edit completions do not support streaming or tool calling.
- See conventions/inception-labs-conventions.yml and errors/inception-labs-problem-types.yml.
