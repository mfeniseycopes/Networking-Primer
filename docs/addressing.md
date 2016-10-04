# Addressing

Addressing provides unique 'addresses' which allows machines to communicate with each other. Each address must not only be unique to the machine, but also unique to the interface which is making/receiving network requests. A way of uniquely identifying each machine/interface is to assign an IP Address to each.

## IP Addresses
IP Addresses are broken up into two types: the older, most common being IPV4 and the new, more robust IPV6.

### IPV4
IPV4 addresses are 32-bit (or a quadruple of bytes) and has a limited address space of 2^32 (4,294,967,296) unique addresses. Each byte has a maximum value of 255, so as a shorthand IPV4 addresses are listed by each byte's numeric value.

Ex.

![](../images/ipv4.png)

### IPV6
IPV6 Addresses are similar to IPV4, but due to the small address space of IPV4, additional addresses were needed. This means more bits per address. IPV6 uses 128-bit and is represented by an octuple of 16-bits. Because 16-bit numbers are quite large (65,536) each part of the octuple is represented using hexadecimal values (base 16).

Ex.

![](../images/ipv6.png)

At the moment, most IP Addresses are still using IPV4 (IPV6 was introduced in 2006 and is not very common). This begs the question, how are more than 8 billion people using an addressing space with just over 4 billion addresses? A single IPV4 address can serve multiple machines/interfaces by using routing, which handles all in/outbound traffic and "routes" it to the correct interface. See [Routing](docs/routing.md) for more info.

## Extra Addressing Fun

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
