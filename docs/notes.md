# Notes

A collection of notes from [Networking for Web Developers](https://www.udacity.com/course/networking-for-web-developers--ud256) by Udacity.

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

## Protocol Layers

Typical Web Application Protocol Stack
```
HTTP > TCP > IP > Hardware
```

Each protocol used when browsing the web is based upon a lower protocol, with the base layer protocol being the hardware interface (wifi, ethernet, lo). Each stacked layer encapsulates and abstracts the layer below it. This means that developers using HTTP don't need to know the details of the TCP protocol but can 'just use it' via the HTTP interface.

**tcpdump** can be used to monitor any network traffic between connections. This can be done by listing the two connecting devices. Ex. `sudo tcpdump -n host 8.8.8.8` monitors data sent between the host machine and 8.8.8.8 and `sudo tcpdump -n port 53` monitors all data sent and received on port 53 (which is the DNS port).

### What happens at the TCP layer when an HTTP request is made

Data sent across a TCP connection is not sent in one large file, but is broken into smaller length packets. These packets are numbered into a sequence and sent across the internet to the recipient, which sends an ACK (or acknowledgement packet) with the sequence number to 'acknowledge' that they received it.

In a typical GET request the initiator sends the GET request packet to the responder. The responder ACKs the request, then sends the data packet(s), each of which has a sequence number. Each data packet is ACKed by the initiator as it comes in. This tells the responder that the packet is received. Since some packets are lost in transmission its important to resend a packet the is not received.

TCP Sequence Diagram

![TCP Sequence Diagram](/images/tcp-sequence.png "TCP Sequence Diagram")

### TCP Flags

Each packet can have single bit flags set to identify certain properties or intents of the package.

* SYN (synchronize) [S] — This packet is opening a new TCP session and contains a new initial sequence number.
* FIN (finish) [F] — This packet is used to close a TCP session normally. The sender is saying that they are finished sending, but they can still receive data from the other endpoint.
* PSH (push) [P] — This packet is the end of a chunk of application data, such as an HTTP request.
* RST (reset) [R] — This packet is a TCP error message; the sender has a problem and wants to reset (abandon) the session.
* ACK (acknowledge) [.] — This packet acknowledges that its sender has received data from the other endpoint. Almost every packet except the first SYN will have the ACK flag set.
* URG (urgent) [U] — This packet contains data that needs to be delivered to the application out-of-order. Not used in HTTP or most other current applications.

### 3-way handshake

The 3-way handshake is how a connection is initialized between a client and server. The client sends an initial packet with the SYN flag set and an initial sequence number. Then the server responds with a packet having a SYN flag,the ACK to the initial sequence number and setting its own initial sequence number. The client then responds with it's next sequence number, ACKing the server's response.

This sets a baseline for the sequence numbers and both client and server acknowledge that they are connected and will send and receive data to eachother.

### 4-way breakup

When one side has finished sending data it will send a FIN flagged packet, telling the other it is done. The other will acknowledge, but continue to send data until it also finishes. At this point it will also send a FIN flagged packet and the side that finished first will acknowledge. At this point both have finished and they will break their connection.

### TCP Congestion

Frequently, fast networks will be sending packets to one another through a slower network. This can easily cause bottlenecks or packet loss. To prevent this TCP slows down the packet send rate each time it does not receive an ACK response. This can be seen by attempting to connect to a closed port.

After enough sends and no ACK responses, the connection will timeout.

Try connecting to `www.udactity.com:12345`.

```bash
# In 1st console
tcpdump -n port 12345

# In 2nd console
nc www.udacity.com 12345
```

## Big Networks

### Hops

As packets move from origin to destination in a network, they are passed from router to router. As they are passed from a router their TTL (time to live) number is decremented by one. When TTL hits zero, the packet is lost and an error message is sent back to the sender with the name of the router where it hit zero.

This is used to prevent infinite loops on a network. If a packet is caught in a loop it will merely decrement to zero and an error will be sent.

*This is actually how traceroute works. By setting the TTL to 0, 1, 2, ... until the destination is hit its able to 'trace' the routing*

### Latency vs Bandwidth

**Latency** is the time it takes data to travel. This is non-dependent on size, but generally dependent upon the number of hops and the distance of those hops. So generally, a New Yorker getting a response from a server in Nanjing would take longer than a server located in Florida.

**Bandwidth** is the amount of data that can be transmitted on a network. It can be thought of as the width of the internet tube that is transmitting data. In this case a two people living in the same building make a request to amazon.com, but one has DSL and the other has gigabit internet, the gigabit connection will receive a largish response quicker. This is because they can be sent more packets per second.

### Middleboxes

A **middlebox** is a device that inspects network traffic and can filter and modify it. These are frequently used to balance server load and prevent unwanted connections.

**Firewalls** are software defined middleboxes installed on the server. These are very common, but can provide problems for webapps reliant on third party connections that may be blocked.

### Web Proxies

A **web proxy** serves as a router for HTTP. This acts as an intermediary between the client and server. Handling requests and making them for the client, then sending the response back to the client. As a middleware they provide extra features like caching and load balancing. They can also send requests to duplicate servers to spread heavy loads across multiple servers.
