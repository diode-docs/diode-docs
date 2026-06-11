---
_schema: default
title: Remote JSON-RPC API
nav_title: Diode API Docs
nav_section: For Your IT Admin
weight: 307
draft: false
---
The Diode Collab API is available from every Diode Collab instance, is device-wide, and is disabled by default.

It is a <a href="https://www.jsonrpc.org/specification" target="_blank" rel="noopener"><strong>JSON-RPC 2.0</strong></a> API for automation and integrations. Requests must use `"jsonrpc": "2.0"` and include a `"method"` field.

**Implementation:** `lib/ddrive_web/remote_rpc.ex` (HTTP), `lib/ddrive_web/rpc_ws.ex` (WebSocket extensions), routing in `lib/ddrive_web/remote_router.ex`.

**Current API version:** returned in successful and error payloads as `data.api_version` (when present)—see `RemoteRPC.version/0` (currently **3\.4**).

---

## Enablement and authentication

1. **Enable the Remote API** on the host (chat: `/sysconfig remote-api enable`; token: `generate-token` / `view-token`).
2. **Bearer token:** every **HTTP** request needs:

   ```
   Authorization: Bearer <your_remote_api_token>
   ```

   If the API is disabled or the token is missing/invalid, the server responds with **401** and a JSON body:

   ```
   {"error":"Unauthorized","reason":"..."}
   ```
3. **WebSocket** (`/api/json_rpc/ws`): connect first, then call **`authenticate`** with the token inside JSON-RPC `params` (see below). Until authenticated, only `authenticate` is accepted for JSON-RPC methods handled by the socket.

---

## Endpoints

| **Transport** | **URL** | **Auth** |
| --- | --- | --- |
| **HTTP POST** | `/api/json_rpc` | `Authorization: Bearer …` |
| **WebSocket** | `/api/json_rpc/ws` | `authenticate` method with `params.token` |

Replace the host with the device’s Diode address, e.g. `https://<device_address>.diode.link/api/json_rpc` (as in chat help templates).

---

## Request shape (all methods)

```
{
  "jsonrpc": "2.0",
  "method": "<method_name>",
  "params": { },
  "id": 1
}
```

* **`params`:** must be a **JSON object** (not an array).
* **`id`:** optional for JSON-RPC; this server may return an empty HTTP body when `id` is omitted (see `rpc_respond?/2`).

---

## Response shape

Success:

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": ...
}
```

Error (typical):

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": "-32602",
    "message": "Invalid params",
    "data": {
      "error": "<atom_name>",
      "details": "…",
      "api_version": "3.4"
    }
  }
}
```

Common JSON-RPC **`error.code`** strings from this API:

| **Code** | **Meaning** |
| --- | --- |
| `-32600` | Invalid request (missing/invalid JSON-RPC envelope) |
| `-32601` | Method not found, or method not allowed on this transport |
| `-32602` | Invalid params (missing key, bad address, unknown zone, etc.) |
| `-32603` | Internal / business error (send failed, forbidden role, etc.) |
| `-32001` | Not authorized (WebSocket, before `authenticate`) |

---

## Shared parameters

### `zone_id` (hex string)

The **drive id** of the zone as `0x` + 40 hex characters. It must match a zone that exists on the host (`Model.App.path/1`). Used by: `send_message`, `list_zone_members`, `add_zone_member`, `remove_zone_member`, and WebSocket helpers that take a zone.

### Resolving channels for `send_message`

* **`channel_id`:** hex string (`0x` + 40 hex), a known group/channel in that zone.
* **`user_name`:** BNS name; resolved to a DM channel with that user in the zone. <br>Provide **`channel_id` OR `user_name`**, not both as conflicting requirements—see implementation in `check_channel/2`.

---

## HTTP methods (`execute_rpc` / POST)

These are routed through `DdriveWeb.RemoteRPC.execute_rpc/2` with source `:post_req`.

### `ping`

Connectivity check.

**Params:** none required (empty object `{}`).

**Example:**

```
curl -s -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"jsonrpc":"2.0","method":"ping","params":{},"id":1}' \
  https://<device>.diode.link/api/json_rpc
```

**Result (string):** `"The RPC endpoint has been successfully contacted."`

---

### `send_message`

Send a chat message to a channel or DM in a zone.

**Params:**

| **Field** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| `zone_id` | string | yes | Zone drive id (hex) |
| `message_text` | string | yes | Message body |
| `channel_id` | string | one of channel | Group id (hex) |
| `user_name` | string | one of channel | BNS name for DM |

**Example (channel):**

```
{
  "jsonrpc": "2.0",
  "method": "send_message",
  "params": {
    "zone_id": "0x…",
    "channel_id": "0x…",
    "message_text": "Hello from JSON-RPC"
  },
  "id": 1
}
```

**Result (string):** `"Message sent"`

**Errors (`data.error`):** include `:invalid_zone_id`, `:zone_not_found`, `:param_key_not_found`, `:channel_not_found`, `:invalid_channel_id`, `:message_send_failed`, etc.

---

### `list_zone_members`

List contract-backed team members for a zone.

**Authorization:** caller must have **Owner** or **Admin** role in that zone (`Model.Folder.myrole/1`). Otherwise `data.error` is `:forbidden`.

**Params:**

| **Field** | **Type** | **Required** |
| --- | --- | --- |
| `zone_id` | string | yes |

**Example:**

```
{
  "jsonrpc": "2.0",
  "method": "list_zone_members",
  "params": { "zone_id": "0x…" },
  "id": 1
}
```

**Result (object):**

```
{
  "members": [
    {
      "member_id": "0x…",
      "role": "Owner",
      "device_ids": ["0x…", "0x…"]
    }
  ]
}
```

`role` is a short string (e.g. `Member`, `Admin`, `Owner`). `device_ids` are linked device addresses for that identity.

---

### `add_zone_member`

Add a member to the zone using the same path as the Team UI (`Model.Membership.add_peer/3`: on-chain add, New Users group, subzone auto-add when applicable).

**Authorization:** **Owner** or **Admin** only (`:forbidden` otherwise).

**Params:**

| **Field** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| `zone_id` | string | yes | Zone drive id (hex) |
| `peer_id` | string | yes | Address to add (hex) — should be the user’s **identity** address (as when picking a team member) |
| `role` | string | no | `Member` (default), `Reader`, `Admin`, or `BackupBot` (case-insensitive). **Owner** is not allowed. |

**Example:**

```
{
  "jsonrpc": "2.0",
  "method": "add_zone_member",
  "params": {
    "zone_id": "0x…",
    "peer_id": "0x…",
    "role": "Reader"
  },
  "id": 1
}
```

**Result (string):** `"Member added"`

**Errors:** `:invalid_peer_id`, `:forbidden`, `:invalid_role`, `:add_member_failed` (with `details` for contract/tx failure), same zone validation as other zone methods.

---

### `remove_zone_member`

Remove a peer from the zone using the same path as the Team UI (`UI.remove_peer/2`).

**Authorization:** **Owner** or **Admin** only (`:forbidden` otherwise).

**Params:**

| **Field** | **Type** | **Required** |
| --- | --- | --- |
| `zone_id` | string | yes |
| `peer_id` | string | yes — member **or** device address (hex) |

**Example:**

```
{
  "jsonrpc": "2.0",
  "method": "remove_zone_member",
  "params": {
    "zone_id": "0x…",
    "peer_id": "0x…"
  },
  "id": 1
}
```

**Result (string):** `"Member removed"`

**Errors:** `:invalid_peer_id`, `:forbidden`, `:remove_member_failed` (with human-readable `details`), zone/channel style errors from shared validation.

---

### `authenticate` (HTTP)

**Not available over HTTP.** Reserved for WebSocket (returns method not available / `-32601`\-style payload). Use **`Authorization`** on POST instead.

---

## WebSocket-only behavior (`/api/json_rpc/ws`)

Handled in `DdriveWeb.RpcWs`. After connecting, authenticate:

### `authenticate`

**Params:**

| **Field** | **Type** | **Required** |
| --- | --- | --- |
| `token` | string | yes — must match `Model.App.remote_api_token()` |

**Example:**

```
{
  "jsonrpc": "2.0",
  "method": "authenticate",
  "params": { "token": "<same_as_bearer_token>" },
  "id": 1
}
```

**Result (string):** `"Authentication successful"`

Until this succeeds, other methods receive an error with `data.error: "not_authenticated"` (code `-32001`).

---

### WebSocket-specific methods

These are **not** handled by `RemoteRPC.execute_rpc/2` for HTTP POST; they exist only on the WebSocket.

#### `subscribe_channel`

Subscribe to new messages for one channel; server will push formatted message batches (see push format below).

**Params:** `zone_id`, and `channel_id` **or** `user_name` (same resolution as `send_message`).

**Result (string):** `"Subscribed"`

---

#### `subscribe_all_channels_in_zone`

Subscribe to all channels in the zone for push notifications.

**Params:** `zone_id`

**Result (string):** `"Subscribed"`

---

#### `get_messages`

Query messages with optional filters.

**Params:**

| **Field** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| `zone_id` | string | yes | Zone drive id |
| `channel_id` | string | no | Limit to one group |
| `user_name` | string | no | DM channel via BNS |
| `timestamp` | integer | no | Unix time in **seconds** — messages after this |
| `limit` | integer | no | Max messages |
| `order` | string | no | `ASC` or `DESC` |

**Result (object):** `{ "messages": [ … ] }`<br>Each element matches the push format (attributes, `creation_time`, `group_id`, `sender`, etc.)—see `RpcWs.format_message/2`.

---

#### `get_members_meta`

Returns last-chat metadata for zone peers (excluding the current user), for UI-style “member list” insights.

**Params:** `zone_id`

**Result (object):**

```
{
  "members": [
    { "name": "…", "last_chat_timestamp": 1234567890 }
  ]
}
```

(`last_chat_timestamp` may be adjusted when the last message was from the local device—see implementation.)

---

### WebSocket: methods also available after auth

After `authenticate`, the same **`ping`**, **`send_message`**, **`list_zone_members`**, **`add_zone_member`**, and **`remove_zone_member`** calls are dispatched through `RemoteRPC.execute_rpc/2` with a WebSocket source. For **`authenticate`** on an already-authenticated socket, the implementation returns: `"You are already authenticated."`

**Push notifications:** when subscribed, the server sends JSON-RPC–style payloads with a `result` containing `{ "messages": [ … ] }` (no request `id` in outgoing pushes—see `prepare_response` usage in `relay_new_messages`).

---

## Further reading

* External product/docs portal (examples linked in validation errors): <a href="https://app.docs.diode.io/" target="_blank" rel="noopener">app.docs.diode.io</a>
* JSON-RPC spec: <a href="https://www.jsonrpc.org/specification" target="_blank" rel="noopener">jsonrpc.org</a>