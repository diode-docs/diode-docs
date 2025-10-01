---
_schema: default
title: Local Machine Proxy Bindings
nav_title: Local Machine Proxy Bindings
nav_section: For Your IT Admin
weight: 303
draft: false
---
Diode Collab runs a local Web3 proxy server on port 57861 that can be used to access Diode Network endpoints - for example, remote SSH, Web3 URLs, and also Web2 URLs as if they are in the USA.

This "works" as if you are running the Diode CLI, but you don't have to run the Diode CLI. The equivalent Diode CLI command is something like: `diode -bind 57861:<exit node address>:1080 socksd`.

So, in order to remote SSH to a machine while running Diode Collab (without running the Diode CLI), you can:

`ssh -o "ProxyCommand=nc -X 5 -x localhost:57861 %h %p" diode@0x00000111122220000111122223333aaaa120e403f.diode`

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;