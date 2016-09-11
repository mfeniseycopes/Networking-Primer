# Networking for Web Developers

## From Ping to HTTP

### Setup

Install:
* **netcat (nc)** - utility for TCP, UDP or UNIX sockets. sends string over the network, which in considered an HTTP request by a web server
* **tcpdump** - prints out a descriptions of networks packets on a network interface
* **traceroute** - tracks the route packets take from an IP network
* **mtr** - investigates network connection between self and host by sending packages and examining routing and accuracy
* **ping** - determines if able to connect to a given address

### Packets

**Ping vs HTTP** - HTTP makes request to server, which then sends a response back. Ping merely sends an echo request to the *operating system* at the address, which sends a echo response back. Ping does not require a server.

**printf** - formatted string

### Layers

**Application** - Protocols: HTTP, SSH, used for URLs, passwords
**Transport** - TCP, UDP, used for port numbers, sessions
**Internet** - IP, used for IP addresses, routes
**Hardware** - wifi, ethernet, DSL, used for signal strength, access points

### Listening

Listening on a port is like having a direct phone line setup. This gives other machines a way to connect to you directly.

`EOF` == `^D`

* can listen on ports 10240-65355 with non-root access, use `sudo` to access lower ports
* can only have one listening process per port
* multiple connections are handled by spawning child processes
* **lsof** - lists open files and network sockets (tells which processes are listening on what ports)
	* `-i` flag shows internet connections

## Names and Addresses

**Host** - a machine on the internet that might host services
**Endpoint** - one end of communication between two machines

**ping** - connects to url via ip address, `-c <num>` to run specific number of connections

### DNS

* **DNS** is a service that looks up an IP for a given domain name
* **A Record** - address record stored at DNS server
* DNS lookup takes places in the OS, which has a built in 'resolver' which returns an IP for a domain name
* in most cases it looks up the IP
* `host` command returns list of aliases for domain name (if available) and the ultimate IP address

### DNS Record Types

* A - maps a domain name to the IP (IPV4) address of a computer hosting the domain
* CNAME - Canonical Name. Used to alias to another domain
* AAAA (quad-A) - (IPV6) domain -> IP mapping
* NS - server component of DNS, tells which DNS servers have records for that domain

### DNS Distribution

Nesting example:
'A' record for www.greenspaceghost.com
* resides in -> 'NS' for greenspaceghost.com
	* resides in -> 'NS' for .com (global tld)
		* resides in -> 'NS' on root server

Browsers typically communicate with a locale caching server (home router or ISP). If the caching server has the IP, it is returned. Otherwise it recursively gets the IP starting at the root server, getting the gTLD from the root, getting the domain name server from the gTLD server, and lastly getting the IP from the domain name server.

```
client -->

caching server --(example.com)--> root --(com NS)-->

caching server --(example.com)--> com --(example.com NS)-->

caching server --(example.com)--> example.com --(IP)-->

caching server --> client
```

**TTL** (Time To Live) - how long a cached DNS record is stored in a caching server

## Addressing and Networking

### Reserved IP Addresses

Some numbers in the 'quad' that makes up IPv4 addresses are reserved, others are not for regular use. For example, 225-240 is used for IP multicasting and 240-255 is 'intended for future use'.

### Netblocks

Computers working on a network can designate a 'network prefix' that is some number of bits in their IP address that is shared. The bits following designate the machine on the network.

A 24 bit prefix, denoted `/24`:
```
Network Prefix   Machine #
[1100 0101 0001] 0001
...
[1100 0101 0001] 1111
```

### Subnet masks

To tell computers how large the network prefix is, a subnet mask is applied. This is a dotted quad with 1s on the prefix and 0s on the machine IP.
```
255.255.255.0
OR
11111111.11111111.11111111.00000000
```

### Interfaces

IP addresses are not assigned to hosts, but to interfaces on that host. For example, a laptop might have 3 interfaces, ethernet (eth), wifi and lo (loopback interface).

**Loopback interface** is used on a local machine to simulate network traffic. ie localhost

### Routers

A router allows interfaces on different networks to communicate with eachother. Each router has a default IP that it uses to connect to the internet. Each subnetwork on it uses the router IP as a default IP.

This chain can continue up a number of routers until it reaches a defaultless router, probably at the ISP level, where it *actually* connects to the internet.

### NAT

Each household is given a single IPv4 address by the IP, the router is directly accessible through this IP and handles data transfer from it's devices by assigning them each unique addresses from the reserved addresses list. Typically 192.168.1.1

**NAT (Network Address Translation)** is the system the router uses to send data from it's inside addresses to outside addresses. This is maintained by a map of which inside addresses & ports are connected to what outside addresses & ports.

Ex.
```
192.168.0.1:80 <--> 206.190.36.45:443
```
NAT is a workaround that makes developing web applications very hard because we don't simply know the unique IP address at a device.
