---
_schema: default
title: Use SSH via Diode on the public Internet?
nav_title: Use SSH via Diode on the public Internet?
nav_section: FAQ
weight: 20014
draft: false
---
Although it is the safest to not expose any surface at all to the Internet, SSH can be one of the safest interfaces to expose - it is made for that purpose. There are many articles on the Internet, such as<a href="https://www.redhat.com/sysadmin/eight-ways-secure-ssh" target="_blank" rel="noopener"><strong> this one</strong></a> from RedHat, that describe some things you can do to make your SSH setup as secure as possible.

The <a href="https://cli.docs.diode.io/docs/using/remote-ssh/" target="_blank" rel="noopener"><strong>SSH example</strong></a> in the Diode docs uses "publish -public" to allow remote SSH into a computer:

```
diode publish -public 22:22
```

If you want to further secure your system, you can publish your SSH port as `private` (only the specific client can gain access) or `protected` (only other clients listed in your [**Fleet Contract**](https://network.docs.diode.io/docs/features/what-is-a-fleet-contract/) can gain access).