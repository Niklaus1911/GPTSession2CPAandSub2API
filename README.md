# ChatGPT Session to CPA / sub2api / Cockpit / 9router / Codex / AxonHub / Codex-Manager

A browser-only single-page tool that converts ChatGPT Web session JSON into importable JSON for CPA, sub2api, Cockpit Tools, 9router, Codex `auth.json`, AxonHub, or Codex-Manager.

## Online Use

### [**Open the converter**](https://gtxx3600.github.io/GPTSession2CPAandSub2API/)

## Usage Notes

Plus accounts can use this conversion flow for compatible relay tools. Free-account access tokens cannot call GPT model APIs.

This tool can help when Codex OAuth sign-in requires phone verification. A Plus account's ChatGPT Web session can be converted into account JSON that relay tools can import. ChatGPT Web sessions usually do not include a `refresh_token`, but the `access_token` lifetime is often long enough for short-term use.

Sessions captured before or after a Plus upgrade work the same way for conversion. A session captured while the account was still Free may carry a Free account-level marker, but once the account currently has Plus enabled, the converted account can call supported model APIs through compatible tools.

This tool is primarily intended for Plus accounts. Free accounts may convert successfully, but they still do not have permission to call GPT models. Join the Discord community for GPT account/resource updates and usage tips for importing converted accounts into CPA or sub2api.

## Discord Community

### [**Join the Discord community**](https://discord.gg/GFmHY2TZNy)

Invite link: `https://discord.gg/GFmHY2TZNy`

## Supported Inputs

You can paste or drag in ChatGPT Web session JSON, for example data containing:

- `user.email`
- `accessToken`
- `sessionToken`
- `expires`
- `account.id`
- `account.planType`

You can also paste or drag in 9router Codex OAuth JSON containing fields such as `accessToken`, `refreshToken`, `expiresAt`, `providerSpecificData.chatgptAccountId`, and `providerSpecificData.chatgptPlanType`.

Native Codex `auth.json` input is supported when it contains fields such as `auth_mode`, `OPENAI_API_KEY`, `tokens.access_token`, `tokens.refresh_token`, `tokens.id_token`, `tokens.account_id`, and `last_refresh`.

AxonHub Codex `auth.json` input is supported when it contains fields such as `tokens.access_token`, `tokens.refresh_token`, `tokens.id_token`, and `last_refresh`.

Codex-Manager batch import JSON is supported when it contains fields such as `tokens.access_token`, `tokens.refresh_token`, `tokens.id_token`, and `meta.label`.

The page also attempts to derive email, account ID, user ID, plan type, and expiration time from the `accessToken` JWT payload.

## Output Formats

- `CPA`: Generates Codex CPA auth JSON with fields such as `type: "codex"`, `access_token`, `session_token`, `id_token`, `email`, `account_id`, plan, and expiration values. When a real `id_token` is missing, the tool builds placeholder JWT claims that Codex can parse from the session and access token claims.
- `sub2api`: Generates the `exported_at/proxies/accounts` structure used by the `CPA2sub2API` project. The account platform is `openai`, the type is `oauth`, and each account object includes `expires_at` and `auto_pause_on_expired`. The account-level `expires_at` comes from the access token JWT `exp` Unix timestamp.
- `Cockpit`: Generates the flat token format recognized by Cockpit Tools Codex JSON import, including `id_token`, `access_token`, `refresh_token`, `account_id`, `email`, and `expired`.
- `9router`: Generates 9router Codex OAuth JSON with fields such as `accessToken`, `refreshToken`, `expiresAt`, `providerSpecificData`, `provider`, `authType`, `priority`, `isActive`, `createdAt`, and `updatedAt`.
- `Codex`: Generates native Codex `auth.json` with `auth_mode: "chatgpt"`, `OPENAI_API_KEY: null`, `tokens.id_token/access_token/refresh_token/account_id`, and `last_refresh`. When a real `refresh_token` is missing, the tool keeps an empty string; the access token cannot refresh automatically after it expires.
- `AxonHub`: Generates AxonHub Codex `auth.json` with `auth_mode: "chatgpt"`, `last_refresh`, and `tokens.access_token/refresh_token/id_token`. When a real `refresh_token` is missing, the tool writes the `__missing_refresh_token__` placeholder for trial use before the access token expires; it cannot refresh automatically after expiration.
- `Codex-Manager`: Generates Codex-Manager batch import JSON with `tokens.access_token/refresh_token/id_token` and `meta.label/workspace_id/chatgpt_account_id/note`. When a real `refresh_token` is missing, the tool keeps an empty string so Codex-Manager does not mistake the account for a refreshable one.

ChatGPT Web sessions usually do not include the OAuth-style `refresh_token`, so access tokens cannot refresh automatically after expiration.

## Local Use

Open this file directly in a browser:

```text
docs/index.html
```

All parsing and conversion happens locally in your browser. Tokens are not uploaded and nothing is written to local storage.
