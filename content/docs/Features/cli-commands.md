---
_schema: default
title: CLI Commands
nav_title: CLI Commands
nav_section: Features
weight: 51
draft: false
---
## **Overview**

If you have the Diode Client installed, in a terminal window, you can type `diode` to get a list of commands and `diode <command> --help` to get detailed help via the command line.

```
Name
  diode - Diode network command line interface

SYNOPSYS
  diode [-allowlists=] [-api=false] [-apiaddr=localho...]
        [-bind=] [-blocklists=] [-blockprofile=] [-blockprofilerate=1] [-bnscachetime=10m0s]
        [-configpath=] [-cpuprofile=] [-dbpath=/Users/...] [-debug=false]
        [-diodeaddrs=] [-e2etimeout=15s] [-fleet=] [-logdatetime=false]
        [-logfilepath=] [-memprofile=] [-metrics=false] [-mutexprofile=]
        [-mutexprofilerate=1] [-pprofport=0] [-retrytimes=3] [-retrywait=1s]
        [-rlimit_nofile=0] [-timeout=5s] [-update=true] COMMAND <args>

COMMANDS
  bns          Register/Update name service on diode blockchain.
  config       Manage variables in the local config store.
  fetch       Fetch is the command to make http GET/POST/DELETE/PUT/OPTION request through diode network.
  gateway      Enable a public gateway server as is used by the "diode.link" website
  publish      Publish ports of the local device to the Diode Network.
  reset        Initialize a new account and a new fleet contract in the network. WARNING deletes current credentials!
  socksd       Enable a socks proxy for use with browsers and other apps.
  time         Lookup the current time from the blockchain consensus.
  token        Transfer DIODEs to the given address on diode blockchain.
  update       Force updating the diode client version.
  version      Print the diode client version.

Run 'diode COMMAND --help' for more information on a command.
```

## Commands

<u>Usage:</u>

> diode \{options\} \{command\} \{args\}

See below for options - they must be specified before the command.

<u>Example:</u>

> diode -debug=true publish -public 80:80

### **diode bns**

```
Name
  diode bns -  Register/Update name service on diode blockchain.

SYNOPSYS
  diode bns <args>

ARGS
  -account string
    	Display the account information of a given BNS name
  -force
    	Force re-registration in case of registration, even if the name is already registered.
  -lookup string
    	Lookup a given BNS name.
  -register string
    	Register a new BNS name with <name>=<address>.
  -transfer string
    	Transfer an existing BNS name with <name>=<new_owner>.
  -unregister string
    	Free a new BNS name with <name>.
EXAMPLE
  diode bns -register hello-world=0x......
```

### **diode config**

```
Name
  diode config -  Manage variables in the local config store.

SYNOPSYS
  diode config <args>

ARGS
  -delete value
    	deletes the given variable from the config
  -list
    	list all stored config keys
  -set value
    	sets the given variable in the config
  -unsafe
    	display private keys (disabled by default)
EXAMPLE
  diode config -delete lvbn2 -delete lvbn
```

### **diode fetch**

```
Name
  diode fetch - Fetch is the command to make http GET/POST/DELETE/PUT/OPTION request through diode network.

SYNOPSYS
  diode fetch <args>

ARGS
  -data string
    	The http body that will be transfered.
  -header value
    	The http header that will be transfered.
  -method string
    	The http method (GET/POST/DELETE/PUT/OPTION). (default "GET")
  -output string
    	The output file that keep response body.
  -url string
    	The http request URL.
  -verbose
    	Print more information about the connection.
EXAMPLE
 diode fetch -method post -data "{'username': 'test', password: '123456', 'csrf': 'abcdefg'} -header 'content-type:application/json'"
```

### **diode gateway**

```
Name
  diode gateway -  Enable a public gateway server as is used by the "diode.link" website

SYNOPSYS
  diode gateway <args>

ARGS
  -additional_ports string
    	httpsd secure server ports
  -allow_redirect
    	allow redirect all http transmission to httpsd
  -certpath string
    	Pem format of certificate file path of httpsd secure server (default "./priv/cert.pem")
  -edge_acme
    	allow to use ACME generate certificates automatically
  -edge_acme_email string
    	ACME email configuration
  -fallback string
    	how to resolve web2 addresses (default "localhost")
  -httpd_host string
    	host of httpd server listening to (default "127.0.0.1")
  -httpd_port int
    	port of httpd server listening to (default 80)
  -httpsd_host string
    	host of httpsd server listening to (default "127.0.0.1")
  -httpsd_port int
    	port of httpsd server listening to (default 443)
  -privpath string
    	Pem format of private key file path of httpsd secure server (default "./priv/priv.pem")
  -proxy_host string
    	host of socksd proxy server (default "127.0.0.1")
  -proxy_port int
    	port of socksd proxy server (default 1080)
  -secure
    	enable httpsd server
  -socksd
    	enable socksd proxy server
EXAMPLE
  diode gateway -httpd_port 8080 -httpsd_port 443 -secure -certpath ./cert.pem -privpath ./priv.pem
```

### **diode publish**

```
Name
  diode publish -  Publish ports of the local device to the Diode Network.

SYNOPSYS
  diode publish <args>

ARGS
  -http
    	enable http static file server
  -http_dir string
    	the root directory of http static file server
  -http_host string
    	the host of http static file server (default "127.0.0.1")
  -http_port int
    	the port of http static file server (default 8080)
  -indexed
    	enable directory indexing in http static file server
  -private value
    	expose local ports to private users.  value may be <local_port>:<remote_port> or <accessible ip>:<port>:<remote_port>
  -protected value
    	expose local ports to protected users (members of fleet contract).  value may be <local_port>:<remote_port> or <accessible ip>:<port>:<remote_port>
  -proxy_host string
    	host of socksd proxy server (default "127.0.0.1")
  -proxy_port int
    	port of socksd proxy server (default 1080)
  -public value
    	expose ports to public users.  value may be <local_port>:<remote_port> or <accessible ip>:<port>:<remote_port>
  -socksd
    	enable socksd proxy server
EXAMPLE
  diode publish -public 80:80 -public 8080:8080 -protected 3000:3000 -protected 3001:3001 -private 22:22,0x......,0x...... -private 33:33,0x......,0x......, -private 192.168.0.123:80:8008
```

### **diode reset**

```
Name
  diode reset -  Initialize a new account and a new fleet contract in the network. WARNING deletes current credentials!

SYNOPSYS
  diode reset <args>

ARGS
  -experimental
    	send transactions of fleet deployment and device allowlist at seme time
EXAMPLE
  diode reset
```

### **diode socksd**

```
Name
  diode socksd -  Enable a socks proxy for use with browsers and other apps.

SYNOPSYS
  diode socksd <args>

ARGS
  -fallback string
    	how to resolve web2 addresses (default "localhost")
  -socksd_host string
    	host of socks server listening to (default "127.0.0.1")
  -socksd_port int
    	port of socks server listening to (default 1080)
EXAMPLE
  diode socksd -socksd_port 8082 -socksd_host 127.0.0.1
```

### **diode time**

```
Name
  diode time -  Lookup the current time from the blockchain consensus.

SYNOPSYS
  diode time <args>

ARGS
  Empty
EXAMPLE
  diode time
```

### **diode token**

```
Name
  diode token -  Transfer DIODEs to the given address on diode blockchain.

SYNOPSYS
  diode token <args>

ARGS
  -balance
    	Just check the balance and quit.
  -data string
    	Data that will be submitted with the transaction.
  -gas string
    	Transfer gas paid to diode miner. (default "21000")
  -gasprice string
    	Transfer gas price paid to diode miner.
  -to string
    	The address or BNS name that DIODEs will be transfered to.
  -value string
    	Amount of DIODEs to be transfered.
EXAMPLE
  diode token -to 0x...... -value 1millidiode -gasprice 10gwei
```

### **diode update**

```
Name
  diode update -  Force updating the diode client version.

SYNOPSYS
  diode update <args>

ARGS
  Empty
EXAMPLE
  diode update
```

### **diode version**

```
Name
  diode version -  Print the diode client version.

SYNOPSYS
  diode version <args>

ARGS
  Empty
EXAMPLE
  diode version
```

## **Options**

<u>Usage:</u>

> diode \{options\} \{command\} \{args\}

Options must be specified before the command.

<u>Example:</u>

> diode -debug=true publish -public 80:80

### **\-diodeaddrs**

Specify a Relay Node to connect / route through.  The client's activity will route through the Relay Node specified.

<u>Args:</u>

> \-diodeaddrs=\[node IP\]:\[node edge port\]

<u>Example usage:</u>

Fetch time from a specified Relay Node

> diode -diodeaddrs=45.79.115.246:41046 time

<u>Other information:</u>

* If another client specifies a different Relay Node to connect through, that client will not be able to connect to your client.
* The \[node IP\] argument may also be a DNS name (e.g. us1.prenet.diode.io)
* The \[node edge port\] defaults to 41046 for most nodes, but some nodes may use a different port as specified by the operator
* A node's \[node IP\]:\[node edge port\] settings can be viewed via the node system's terminal by "sudo snap get diode-node".  The "host" parameter is the \[node IP\], and the "edge2-port" parameter shows the available edge ports (choose one to specify).  The settings are also visible in the node's web dashboard under "Primary Edge Address".

### **\-bind**

Bind a local port to a remote client's Web3 port.  Activity into your local port will emerge on the remote client's port.

<u>Args:</u>

> \-bind \[local port\]:\[client ID\]:\[remote port\]:\[udp\|tcp\|tls\]

*bind's* transport default is "tcp"

<u>Example usage:</u>

Bind the local port 15900 to a remote client's local port 5900 (as often used for secure VNC tunneling).

> diode -bind 15900:0x123fc642e256e1069caf9c3a812cf314c623abcd:5900

<u>Other information:</u>

* [Remote VNC example](https://cli.docs.diode.io/docs/using/remote-vnc-articles/)
* [Remote SMB/Samba share example](https://cli.docs.diode.io/docs/using/tunneled-p2p-dashboards/)

### **\-debug**

View more verbose debug messages in the console

<u>Args:</u>

> \-debug=\[true \| false\]

*debug* default is "false"

<u>Example usage:</u>

Fetch time from the network while showing verbose debug messages.

> diode -debug=true time

### **\-dbpath**

Specify a different database than the default.  This can be used to run multiple Diode identities  via the CLI on the same machine.

<u>Args:</u>

> \-dbpath=\[path to database file\]

<u>Example usage:</u>

Use a database on the Desktop to publish content to that database's identity via Web3.

> diode -dbpath=/home/pi/Desktop/private.db publish -public 80:80

<u>Other information:</u>

* [Use a different wallet for the CLI](https://cli.docs.diode.io/docs/faq/how-do-i-use-a-different-wallet-for-the-diode-cli/)
* [Where is my wallet stored?](https://cli.docs.diode.io/docs/faq/where-is-my-local-wallet-stored/)

### **\-fleet**

Specify a different fleet address than is stored in the configuration.  Useful for testing.

<u>Args:</u>

> \-fleet=\[fleet address\]

<u>Example usage:</u>

Publish content using the development fleet contract.

> diode -fleet=0x6000000000000000000000000000000000000000 publish -public 80:80

<u>Other information:</u>

* [What is a fleet contract?](https://network.docs.diode.io/docs/features/what-is-a-fleet-contract/)
* The development fleet contact (address 0x6000000000000000000000000000000000000000) is a "promiscuous" fleet contract that is available for free for all devices to use.  It is also the default fleet contract used by new installations of the Diode CLI.  It does not have any SLA guarantees due to low or no incentives for the network to route its traffic.
* [Persist a different fleet](https://network.docs.diode.io/docs/faq/change-my-fleet-perimeter/)

### **\-bnscachetime**

Specify a name cache staleness timeout so that any subsequent use of the name (after the timeout) refreshes the name members from the chain.  Useful when using BNS names as access control lists that may frequently change.

<u>Args:</u>

> \-bnscachetime=\[timeout\]

*timeout* default is "10m0s"

<u>Example usage:</u>

Publish private content to all clients listed as members of the "my-auth-list" name, and refresh the member list every five minutes.

> diode -bnscachetime=5m0s publish -private 80:80,my-auth-list

&nbsp;

&nbsp;