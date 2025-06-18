---
_schema: default
title: 'Can''t SSH - nc: read failed'
nav_title: 'Can''t SSH - nc: read failed'
nav_section: FAQ
weight: 20002
draft: false
---
The Diode CLI 1.15.1 and earlier has a known bug that can be exposed upon an unexpected hard shut down of the client.  This bug is related to the client optimistically storing known good block information, but not being able to validate the information before the shutdown occurred.

This is often seen when trying to SSH into the device where the SSH connection will respond with:

> nc: read failed (2/4): Broken pipe <br>Connection closed by UNKNOWN port 65535

On the device, errors such as the following will be seen:

> ERROR Couldn't add block 9235357 \[0 0 1 177 32 141 120 159 149 175 34 137 29 206 225 187 242 113 118 96 51 102 189 102 74 44 101 82 73 161 198 161\]: don't know direct parent of this block server=us2.prenet.diode.io:41046

This can be fixed by clearing the lvbh3 and lvbn3 configuration variables, thereby forcing the use of Blockquick's long-validation algorithm to rebuild its reputation table.  On a terminal on the impacted device, execute:

> diode config -delete lvbn3 -delete lvbh3

&nbsp;