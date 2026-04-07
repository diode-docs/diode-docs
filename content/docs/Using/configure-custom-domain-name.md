---
_schema: default
title: Configure Custom Domain Name
nav_title: Configure Custom Domain Name
nav_section: Using
weight: 105
draft: false
---
Diode can be used to host content on a custom web2 <a href="https://web.archive.org/web/20241011054759/https://en.wikipedia.org/wiki/Top-level_domain" target="_blank" rel="noopener">Top Level Domain</a> (TLD) - similar to <a href="https://web.archive.org/web/20241011054759/https://pages.github.com/" target="_blank" rel="noopener">github pages</a>. This enables exciting possibilities of edge hosting for websites and other TLD-centric content.

For example, if you’ve registered a domain “[yourname.com](https://web.archive.org/web/20241011054759/http://yourname.com/)”, you can host a website on “[yourname.com](https://web.archive.org/web/20241011054759/http://yourname.com/)” that is served from your diode client (e.g. from a Raspberry Pi, your laptop, or a server).

No static IP address or paid web host is required!

**Important Note:** Diode custom domains match the web2 name BEFORE the TLD suffix to the Diode BNS Name (web3 name). Example: `yourname` BNS entry matches `yourname.com` AND ALSO matches `yourname.net` (and every other `yourname.<tld>`). Therefore, it is “first come first serve” - the first `yourname.<tld>` web2 domain owner that registers `yourname` with Diode will be the operator of the web3 domain.

### **Preparation**

1. Verify the TLD for your customer domain is a two, three, or four letter TLD (e.g. `.com` is a three letter TLD).
2. Register the domain name as a BNS name with Diode and set the “Destination” of your BNS name to the address of the diode client hosting the content. For example, if your web2 domain is [yourname.com](https://web.archive.org/web/20241011054759/http://yourname.com/) then <a href="https://web.archive.org/web/20241011054759/https://network.docs.diode.io/docs/using/reserve-a-domain-name/" target="_blank" rel="noopener">register “yourname” on the diode chain</a>.<br>Or just on the command line:

   ```
   diode bns -register yourname
   ```
3. Create an DNS A record to point your web2 domain to the IP address for diode.link (currently the IP is 45.79.224.174, but you can get the IP by opening a terminal window and typing `ping diode.link`). To do this, you’ll have to go to your DNS provider’s website and edit the DNS records.

[porkbun.com](https://web.archive.org/web/20241011054759/https://porkbun.com/) example:

**![](https://web.archive.org/web/20241011054759im_/https://files.helpdocs.io/qwk5dmv7m8/articles/6pctb40wj8/1605501604227/image.png)**

That’s it! It will take up to an hour for your DNS changes to apply. So, don’t forget to [bring up your website](https://web.archive.org/web/20241011054759/https://cli.docs.diode.io/raspberry-pi/host-a-website-with-ghost/) and start serving your decentralized web!

In the example of `yourname.com`, you should be able to see your content at the URLS:

https://&lt;clientaddress&gt;.diode.link

[http://yourname.diode.link](https://web.archive.org/web/20241011054759/http://yourname.diode.link/)

[http://yourname.com](https://web.archive.org/web/20241011054759/http://yourname.com/)

&nbsp;