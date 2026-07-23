---
name: Converse with the Adapter Life assistant
description: Create a conversation and stream an assistant completion, then read it back.
api: openapi/adapter-openapi.json
operations: [create_conversation_users_me_conversations_post, send_message_users_me_conversations__conversation_id__completion_post, get_conversation_users_me_conversations__conversation_id__get, stop_response_users_me_conversations__conversation_id__stop_response_post]
---

# Converse with the Adapter Life assistant

Use this to drive a chat turn against the Adapter Life API.

## Auth
Send `Authorization: Bearer <JWT>` on every call (HTTPBearer). Base URL `https://api.adapter.com/v1`.

## Steps
1. **Create a conversation** — `POST /users/me/conversations` (`create_conversation_users_me_conversations_post`). You may supply your own UUID so the client can navigate before AI processing starts.
2. **Send a message** — `POST /users/me/conversations/{conversation_id}/completion` (`send_message_users_me_conversations__conversation_id__completion_post`). Default `stream=true` returns Server-Sent Events; pass `stream=false` for a single JSON object.
3. **Reconnect if the stream drops** — `GET /users/me/conversations/{conversation_id}/stream/{session_id}` resumes exactly where you left off (durable, Redis-backed).
4. **Read the conversation** — `GET /users/me/conversations/{conversation_id}` (`get_conversation_users_me_conversations__conversation_id__get`) for the full message history.
5. **Cancel** — `POST /users/me/conversations/{conversation_id}/stop_response` to stop an in-flight generation.

## Rules
- Pagination on list endpoints is `limit`/`offset` (see conventions/adapter-conventions.yml).
- There is no Idempotency-Key contract; do not assume safe automatic retries of writes.
- Errors are FastAPI JSON; 422 carries `detail[]` field errors (see errors/adapter-problem-types.yml).
