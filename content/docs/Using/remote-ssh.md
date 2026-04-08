---
_schema: default
title: Remote SSH
nav_title: Remote SSH
nav_section: Using
weight: 9
draft: false
---
Diode can be used to create a secure connection to a system implementing SSH without requiring a static IP address and without exposing your SSH port (usually port 22) to the public Internet.

[**SSH**](https://en.wikipedia.org/wiki/Secure_Shell) provides a Secure Shell access to many server systems, IT equipment, and devices. It is the defacto way of remotely connecting to an IT system for the purpose of managing and maintaining the asset. However, if the system does not have a globally unique static IP address, or does not expose port 22 (or as configured) to the public Internet, SSH cannot be used to access the system.

**Why remote SSH?**  It is sometimes not desirable to publish a server or system's SSH interface over the public Internet - there are bots constantly probing for SSH connections to hack into.  Also, some ISPs, for example satellite Internet providers, manipulate traffic and break public connections via SSH. By using Diode remote SSH, you can keep your SSH port off the public Internet and can tunnel through the non-compliant ISPs (even if you do have your SSH port publicly accessible).

Have fun with this, and let use know in our [**T**]()[**elegram**](https://t.me/diode_chain) channel if you have any feature requests!

## SSH via Diode ephemeral keys

### SSH Server Setup (diode publish -sshd)

1\. Install the Diode CLI:

```
curl -Ssf https://diode.io/install.sh | bash
```

2\. SSH can only be published privately (allow list) or protected (to other devices in a fleet)

* This example uses private publication - you need to go get a list of devices, or group name, to allowlist.  The allowlist can use a list of Client addresses (e.g. 0xabc..123), BNS names, Diode Collab identity contracts, Diode Collab usernames, or Diode Collab zone names
* Here, we will publish privately to a machine running the CLI with Client address 0x711f0f2c5e9904925e345777b9500f1b70a4cc25

3\. Open a terminal window / shell

4\. Run the Diode CLI sshd service:

```
diode publish -sshd private:22:diode,0x711f0f2c5e9904925e345777b9500f1b70a4cc25
```

* You can choose the Diode Client address port to publish SSH on - we are using the "typical" port 22
* The "diode" is the username on the server - yours might be "root" or "myuser" or whatever
* The "0x711f0f2c5e9904925e345777b9500f1b70a4cc25" is the allow list for which other devices you want to allow to SSH in - you could add other IDs or names separated by commas (no spaces)
* Note the server's Client address (printed out when the CLI starts up) - that is the address that your clients will connect to in the next section.  For our example, the server's Client address is "0x13ca2702af7176f3120ab6f4d3cadcb20d00e1e5"

### SSH Client Setup (diode ssh)

1\. Install the Diode CLI:

```
curl -Ssf https://diode.io/install.sh | bash
```

2\. Open a terminal window

3\. Verify your Diode CLI's Client address is in the server's allow list - you can type "diode time" to see your Client address

4\. SSH in!

```
diode ssh diode@0x13ca2702af7176f3120ab6f4d3cadcb20d00e1e5.diode
```

* The "diode" is the username on the server
* The "0x13ca2702af7176f3120ab6f4d3cadcb20d00e1e5" is your server's Client address

That's it!  As long as your CLI's device address is in the server's allowlist, or in a name that resolves to a list of devices containg your CLI's device address, you will be able to reach your device from anywhere in the world - all without publishing your SSH port on the public Internet!

## Remote SSH with server-managed credentials

You can also simply publish your existing SSH interface publicly or privately with Diode.  On the server, instead of "diode publish -sshd" (which manages ephemeral keys for you), you just:

```
diode publish -public 22:22
```

Of course, you can use -private 22:22,\[allowlist\] or -protected 22:22 instead.

You don't get the advantage of ephemeral credential management, but this approach could be better for some setups.  The SSH client setup is the same as above.

## Start Diode at Boot

If everything works fine we recommend to <a href="https://cli.docs.diode.io/raspberry-pi/start-diode-on-boot/" target="_blank" rel="noopener"><strong>setting up the Diode Client to start at boot time</strong></a> on your SSH Server device.

## Other infos

### More info about "protected"

You can use the "protected" mode to publish SSH. This restricts access to only the systems authorized for the same Fleet Contract as the SSH server system belongs to.

To <a href="https://network.docs.diode.io/docs/features/what-is-a-fleet-contract/" target="_blank" rel="noopener"><strong>set this up</strong></a>, you first have to <a href="https://network.docs.diode.io/docs/faq/configure-metamask/" target="_blank" rel="noopener"><strong>enable MetaMask on your browser</strong></a> (save the private key of the wallet you create so you, or other admins, can administrate the Fleet Contract even if your computer is no longer available!), then create a Fleet Contract at [https://diode.io/prenet/#/fleets](https://diode.io/prenet/#/fleets), then add your SSH Server's Diode client address to the Fleet Contract, and finally configure the SSH Server's Diode client to communicate as a member of the Fleet Contract (via terminal: `diode config -set fleet=<fleet address>`).

After setting up the Fleet Contract, you can use the same process to add and configure the SSH Client systems you would like to access the SSH server (use the Prenet site to add the SSH Client's Diode client address to the Fleet Contract, then configure the SSH Client's Diode client to communicate as part of the Fleet Contract via `diode config -set fleet=<fleet address>`).

Finally, you would run the Diode client on the SSH Server system to publish the SSH interface as "protected":

```
diode publish -protected 22:22
```

### Native SSH Client Usage

On the SSH client device, you can also just use the built-in ssh command to remotely access your SSH server device with:

```
ssh -o "ProxyCommand=nc -X 5 -x diode.link:1080 %h %p" <user>@<client_address>.diode
```

* &lt;user&gt; is your username (e.g. on a Raspberry Pi it will usually be `pi`\- e.g. `pi@<client_address>.diode`)

* &lt;client\_address&gt; is the Client Address you copied in the SSH Server Setup section

Note that you can also skip the diode.link gateway proxy and direct-connect with your client's native ssh client by:

1\. Open two terminal windows on the computer you want to SSH from.

2\. In one terminal window, start the Diode client in socksd mode:

```
diode socksd
```

3\. In the other terminal window, start SSH using the localhost private Diode connection:

```
ssh -o "ProxyCommand=nc -X 5 -x localhost:1080 %h %p" <user>@<client_address>.diode
```

&nbsp;