---
_schema: default
title: 'Developers (Network, CLI): Start Here!'
nav_title: Start Here
nav_section: Using
weight: 3
draft: false
---
Hi - we're glad you want to start using the [**Diode Network**](https://diode.io/solutions/network/) - you won't be disappointed!

### Bandwidth Network

The Diode Network is a bandwidth network.  You can use it to establish point-to-point TCP or UDP connections that are bridged by Diode Network relay nodes.  All bandwidth on the network requires a sponsor and most people use Diode's development fleet contract to get going (it is the default sponsor in the CLI and the ZTNA console).

### CLI

The Diode CLI is a great tool for connecting server systems, server-based apps, and IT personnel machines.

It is a cross platform command line tool written in Go that maps ports to resources and transparently bridges one client to another client via the network.  It installs in seconds and you can<a href="https://cli.docs.diode.io/docs/using/publish-static-content/" target="_blank" rel="noopener"> publish your first content in less than five minutes.</a>

The CLI [docs are here](https://cli.docs.diode.io), <a href="https://github.com/diodechain/diode_client" target="_blank" rel="noopener">github is here</a>.

### ZTNA Console

The ZTNA Console is the primary way that IT personnel manage their configurations of the Diode Network.

It is a SaaS app that provides a highly secure WYSWIG UI and powerful API to configure assets that use the Diode Network.  Unlike other secure networks that introduce vendor risk at the network control plane, the Diode ZTNA Console uses self custody SSO combined with the Oasis Network's TEE (Intel TPM) to ensure your ACLs and definitions are fully private and self-custody.

The ZTNA docs are in the same section as this article.

### Libraries for App Integration

Diode's production communication library, built for integration into other applications, is the elixir Diode Client.  Diode maintains internal builds for Android, iOS, Windows, Mac, and Linux variants built on top of the elixir Diode Client.

For example, [Diode Collab](https://diode.io/products/collab), Diode's cross-platform team collaboration application is built on the elixir Diode Client.

The elixir Diode Client <a href="https://github.com/diodechain/diode_client_ex" target="_blank" rel="noopener">github is here</a>.

### **About Client Identities**

All clients on the Diode Network have a unique address (their "client identity") that they use to interact with the network. They can be used to publish content, access private or protected content, manage Fleet Contracts, and reserve a BNS name (domain name).

Addresses (identities) on the Diode Network are <a href="https://ethereum.github.io/yellowpaper/paper.pdf" target="_blank" rel="noopener"><strong>Ethereum public addresses</strong></a>. In order to instantiate something persistent on the Diode Network, you must have an address, and you must have access to the addresses' private key - typically stored in a wallet somewhere.

Each Diode Client has its own wallet that it manages on the device the client is running on.

### **Read More**

To read more about the Diode Network and related technologies, see the [**Useful Links and Articles**](https://network.docs.diode.io/useful-links-and-articles/).

---

&nbsp;