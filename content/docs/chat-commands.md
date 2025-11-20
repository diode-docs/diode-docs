---
_schema: default
title: Chat Commands
nav_title: Chat Commands
nav_section: Features
weight: 204
draft: false
---
In a Diode Collab channel, messages that start with "/" are called chat commands. These chat commands can perform various functions and display helpful information.

Chat commands and the automated responses to them don't send any data to the Channel members, only the sender can see them, and they disappear after leaving the page.

### **Chat Commands**

#### **`/sysconfig remote-api COMMAND`**

&nbsp;

| **COMMAND** | **Description** |
| --- | --- |
| version | display the current Diode Collab API version |
| enable | enable the Diode Collab API on this device |
| disable | disable the Diode Collab API on this device |
| status | display status of the Diode Collab API on this device |
| generate-token | generate a new bearer token (this replaces any previous token) |
| view-token | view the current bearer token (this is a secret) |
| template | display a curl template for the "send\_message" API method |
| example | auto-populate a curl command for the "send\_message" API method with the current "channel\_id", "zone id", and "device\_address" of the channel/zone/device this command was run on - the bearer token will still need manually filled in |

#### **`/info`**

Outputs the following values:

* ddrive\_version
* device\_address
* zone\_domain
* zone\_id
* channel\_id