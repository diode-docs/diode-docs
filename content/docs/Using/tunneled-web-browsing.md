---
_schema: default
title: Tunneled Web Browsing
nav_title: Tunneled Web Browsing
nav_section: Using
weight: 225
draft: false
---
Diode Collab acts as a secure tunneling browser for teams, enabling access to web apps via end-to-end encrypted tunnels (enterprise VPN-style) or regional exits (consumer VPN-style). This guide focuses on sharing launchable sessions through bookmarks and deep links in Markdown, websites, or browsers, assuming private content is published first.

## Why Deep Links Matter

Teams lose time navigating apps or misclicking wrong links. Deep links fix this and can launch the exact tunneled browsing session (Web3 for private .diode sites, Web2 for geo-specific public sites). No app-switching, no manual config, perfect for distributed teams hitting private dashboards or region-locked tools.

## Publish Private Content First

For Web3 tunneled access to private sites/dashboards, use Diode CLI (`diode publish -private 80:80,<authorized_address>`) or the [Diode Deployer](https://deployer.diode.link/), which is a web tool for wallet-based, private Vibe app deploys with no public exposure. Get the .diode address for use in bookmarks/deep links.

## Create Bookmarks

[Bookmarks](https://app.docs.diode.io/docs/features/diode-bookmarks/) provide one-click launches from your Zone sidebar.

[**Web3 Links**](https://app.docs.diode.io/docs/features/diode-bookmarks/#type-web3-link)(encrypted to .diode addresses):

* Zone &gt; Bookmarks &gt; "+": Name it, select "Web3 Link", choose "Brave" (install/close first) or "Internal" browser.
* Enter Address (e.g., `mysite.diode` or `0x...diode`), set order, Save.

[**Web2 Tunneled**](https://app.docs.diode.io/docs/features/diode-bookmarks/#type-web2-tunneled-link) (regional public sites):

* Select "Web2 Tunneled Link", enter site (e.g., `example.com`), pick Exit (e.g., US West; add custom via CLI).
* Save and share the bookmark name for deep links.

Edit anytime via Zone Settings &gt; Bookmarks.

## Launch via Zone Bookmarks

Click bookmark in sidebar → instant tunneled browser. Share the bookmark *name* for deep links below.

## Embed Deep Links

Markdown example:

```
## Secure Admin Dashboard
[Launch Private Tunnel](ddrive://bookmark
target=diode%3A%2F%2Fsunsethaiku.diode%3A8008&zone=drive-i9jqub11bg8sm6o1luyt)

## Geo-Restricted Tool
[Launch US-West Site](ddrive://bookmark?target=example.com&zone=yourzoneid)
```

Get Zone ID from Settings &gt; Zone Identification; escape targets with % encoding. Clicking opens Collab's tunneled browser.

**Adhoc Deep Links** (no Zone/bookmark setup):

```
[Quick Private Access](ddrive://browser?url=diode%3A%2F%2Fmysite.diode)
[Quick Regional Browse](ddrive://browser?url=https%3A%2F%2Fexample.com&proxy=uswest.diode)
```

Use in emails, Slack, or external sites: diode:// for direct encryption, https:// for regional exit (US West default).

## Pro Tips

* Internal browser = max security (simple sites only); Brave = complex sites (but exit fully first).
* Test links in Markdown previewers to confirm encoding.
* Scale to teams: Invite to Zone, distribute deep links in shared docs.

&nbsp;