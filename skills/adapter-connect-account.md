---
name: Connect a third-party account via OAuth
description: Initiate an OAuth connection for a provider, complete the callback, and list connected accounts.
api: openapi/adapter-openapi.json
operations: [initiate_oauth_oauth2_connect_initiate_post, oauth_callback_oauth2_connect_callback_post, list_connected_accounts_auth_users_me_connected_accounts_get, disconnect_account_auth_users_me_disconnect_account_post]
---

# Connect a third-party account via OAuth

Adapter ingests a user's connected data sources; this skill links one.

## Auth
`Authorization: Bearer <JWT>`. Base URL `https://api.adapter.com/v1`.

## Steps
1. **Initiate** — `POST /oauth2-connect/initiate` (`initiate_oauth_oauth2_connect_initiate_post`) for the target provider; redirect the user to the returned authorization URL.
2. **Complete the callback** — after the provider redirects back, `POST /oauth2-connect/callback` (`oauth_callback_oauth2_connect_callback_post`) with the returned code/state.
3. **Verify** — `GET /auth/users/me/connected-accounts` (`list_connected_accounts_auth_users_me_connected_accounts_get`) to confirm the account is linked.
4. **Disconnect** — `POST /auth/users/me/disconnect-account` (`disconnect_account_auth_users_me_disconnect_account_post`) to remove it.

## Rules
- 401/403 mean the bearer token is missing or not permitted (see errors/adapter-problem-types.yml).
- Distinct from Adapter's own OAuth server (authentication/adapter-authentication.yml), which mints Adapter tokens (mcp:read/offline_access) for external apps.
