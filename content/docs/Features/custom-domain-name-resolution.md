---
_schema: default
title: Custom Domain Name Resolution
nav_title: Custom Domain Name Resolution
nav_section: Features
weight: 115
draft: false
---
Diode can be used to host content on a custom web2 <a href="https://en.wikipedia.org/wiki/Top-level_domain" target="_blank" rel="noopener"><strong>Top Level Domain</strong></a> (TLD) - similar to <a href="https://pages.github.com/" target="_blank" rel="noopener"><strong>github pages</strong></a>. This enables exciting possibilities of edge hosting for websites and other TLD-centric content.

For example, if you've registered a domain "[**yourname.com**](http://yourname.com)**",** you can host a website on "[yourname.com](http://yourname.com)" that is served from your diode client (e.g. from a Raspberry Pi, your laptop, or a server).

No static IP address or paid web host is required!

**Important Note:** Diode custom domains match the web2 name BEFORE the TLD suffix to the Diode BNS Name (web3 name). Example: `yourname` BNS entry matches `yourname.com` AND ALSO matches `yourname.net` (and every other `yourname.<tld>`). Therefore, it is "first come first serve" - the first `yourname.<tld>` web2 domain owner that registers `yourname` with Diode will be the operator of the web3 domain.

### **Preparation**

1. Verify the TLD for your customer domain is a two, three, or four letter TLD (e.g. `.com` is a three letter TLD).
2. Register the domain name as a BNS name with Diode and set the "Destination" of your BNS name to the address of the diode client hosting the content. For example, if your web2 domain is [**yourname.com**](http://yourname.com) then <a href="https://network.docs.diode.io/docs/using/reserve-a-domain-name/" target="_blank" rel="noopener"><strong>register "yourname" on the diode chain</strong></a>.<br>Or just on the command line:

   ```
   diode bns -register yourname
   ```
3. Create an DNS **A record** to point your web2 domain to the IP address for diode.link (currently the IP is 45.79.224.174, but you can get the IP by opening a terminal window and typing `ping diode.link`). To do this, you'll have to go to your DNS provider's website and edit the DNS records.

[**porkbun.com**](https://porkbun.com/) example:

![](/uploads/image-29.png)

That's it! It will take up to an hour for your DNS changes to apply. So, don't forget to [**bring up your website**](https://cli.docs.diode.io/raspberry-pi/host-a-website-with-ghost/) and start serving your decentralized web!

In the example of `yourname.com`, you should be able to see your content at the URLS:

https://&lt;clientaddress&gt;.diode.link

[http://yourname.diode.link](http://yourname.diode.link)

[http://yourname.com](http://yourname.com)

---

&nbsp;