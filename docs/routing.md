# Routing

A router allows provides a unified interface and IP for a collection of interfaces (computers, phones, internet of things devices). Because of the limited number of unique IPs provided by IPv4, routers are necessary to expand the size of the internet beyond its current capacity.

Each household is given a single IPv4 address by their ISP (Internet Service Provider). But in most cases, each household has many devices that connect to the internet. The router takes the IP provided by the ISP and creates a smaller subnetwork containing a set of internet capable devices.

Each router has a default IP that it uses to connect to the internet. Each subnetwork on it uses the router IP as a default IP and is given a local network address from one of the Internet Protocol's reserved ranges.

A router itself can exist on a subnetwork of another router. This chain can continue up a number of routers until it reaches a defaultless router, probably at the ISP level, where it *actually* has a unique IP address and *actually* connects to the internet.

## NAT (Network Address Translation)

NAT is the system the router uses to send data from it's inside addresses to outside addresses. This is maintained by a map of which inside addresses & ports are connected to what outside addresses & ports.

Ex.
```
Local address:port     <- maps to ->   router address:port
192.168.0.1:80         <- maps to ->   206.190.36.45:443
```
NAT is a workaround that makes developing web applications very hard because we don't simply know the unique IP address at a device.
