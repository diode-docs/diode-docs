---
_schema: default
title: Diode WebSocket API Python Example (subscribe_channel)
nav_title:
SEO_options:
  title: Diode App Secure Messenger
  image:
  description:
draft: false
---
This is an example python script demonstrating how to use the Diode API with websockets:

```
import asyncio
import json
import os
import ssl
import sys
import websockets
import websockets.exceptions

bearer_token = "REPLACE"
zone_id = "REPLACE"
channel_id = "REPLACE"
device_id = "REPLACE"
diode_url = f"https://{device_id}.diode.link/api/json_rpc"
diode_wss_uri = f"wss://{device_id}.diode.link/api/json_rpc/ws"
method_send_msg = "send_message"
method_auth = "authenticate"
method_subscribe = "subscribe_channel"
method = method_send_msg


def _ssl_context():
    """SSL context for wss: use certifi CA bundle if available; set DIODE_VERIFY_SSL=0 to disable verification."""
    verify = os.environ.get("DIODE_VERIFY_SSL", "1").strip().lower() not in ("0", "false", "no")
    if verify:
        ctx = ssl.create_default_context()
        try:
            import certifi
            ctx.load_verify_locations(certifi.where())
        except ImportError:
            pass
        return ctx
    return ssl._create_unverified_context()


def _open_timeout():
    """Seconds to wait for opening handshake. Set DIODE_OPEN_TIMEOUT (default 60)."""
    try:
        t = int(os.environ.get("DIODE_OPEN_TIMEOUT", "60").strip())
        return max(5, min(300, t))
    except ValueError:
        return 60


def base_payload():
    return {"jsonrpc": "2.0", "id": 1}


def handle_exception(e):
    if isinstance(e, websockets.exceptions.WebSocketException):
        print(f"WebSocket error: {e}")
    elif isinstance(e, websockets.exceptions.ConnectionClosedError):
        print(f"Conn closed unexpectedly: {e}")
    elif isinstance(e, json.JSONDecodeError):
        print(f"JSON decoding err: {e}")
    elif isinstance(e, asyncio.TimeoutError):
        print(f"Timed out request: {e}")
    else:
        print(f"An unexpected error occurred: {e}")


async def send_auth(websocket):
    # API v3.2 requires params as a JSON object (not array)
    params = {"token": bearer_token}
    payload = base_payload()
    payload["method"] = method_auth
    payload["params"] = params
    message = json.dumps(payload)
    await websocket.send(message)
    response = await websocket.recv()
    return json.loads(response)


async def subscribe_channel(websocket):
    # API v3.2 requires params as a JSON object (not array)
    params = {"zone_id": zone_id, "channel_id": channel_id}
    payload = base_payload()
    payload["method"] = method_subscribe
    payload["params"] = params
    message = json.dumps(payload)
    await websocket.send(message)
    response = await websocket.recv()
    return json.loads(response)


async def send_message(websocket, text):
    # API v3.2 requires params as a JSON object; key is message_text not message
    params = {"zone_id": zone_id, "channel_id": channel_id, "message_text": text}
    payload = {"jsonrpc": "2.0", "id": 2, "method": method_send_msg, "params": params}
    await websocket.send(json.dumps(payload))
    response = await websocket.recv()
    return json.loads(response)


# message receiver
async def receive_messages():
    try:
        print(diode_wss_uri)
        ssl_ctx = _ssl_context()
        open_timeout = _open_timeout()
        print(f"open_timeout={open_timeout}s")
        async with websockets.connect(diode_wss_uri, ssl=ssl_ctx, open_timeout=open_timeout) as websocket:
            try:
                # initial authentication
                resp = await send_auth(websocket)
                print(resp)

                # subscribe to the diode channel
                resp = await subscribe_channel(websocket)
                print(resp)

                # send a hello message
                resp = await send_message(websocket, "hello")
                print("send_message:", resp)

            except Exception as e:
                handle_exception(e)
                sys.exit(1)

            while True:
                message = await websocket.recv()
                print(f"Received message: {message}")

    except KeyboardInterrupt:
        print("Program exit. Interrupted by user")
    except Exception as e:
        print(f"An error occurred upon connection attempt: {e}")


# main loop
asyncio.run(receive_messages())
```