---
_schema: default
title: Can I Assign a New Client Address to an Existing BNS Name?
nav_title: Can I Assign a New Client Address to an Existing BNS Name?
nav_section: FAQ
weight: 20003
draft: false
---
Yes you can. If you use <a href="https://support.diode.io/article/2bzhaehdp2" target="_blank" rel="noopener"><strong>MetaMask to manage the BNS name</strong></a>, you can go to the <a href="https://diode.io/prenet/#/dns" target="_blank" rel="noopener"><strong>Diode Network Explorer's BNS page</strong></a> and simply enter a new Client Address / ID in the box provided under the BNS name. You must be <a href="https://support.diode.io/article/uec3mloh9z" target="_blank" rel="noopener"><strong>authenticated with MetaMask</strong></a> and connected to the account that owns the BNS name.

You can use the Diode Client to do this without MetaMask per:

`diode bns -register <name>=<new_address>`

In order to run this command, the Diode Client running the command must be the original owner of the BNS name.