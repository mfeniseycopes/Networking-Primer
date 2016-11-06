# Networks

Networks allow machines that are not directly connected to exchange data through their neighboring connections. Each connection passed through from origin to destination is called a *hop*.

## Hops

As packets move from origin to destination in a network, they are passed from router to router. As they are passed from a router their *TTL (time to live)* number is decremented by one. The TTL is simply a number representing the maximum number of hops before a transmission fails. When TTL hits zero, the packet is considered lost and an error message is sent back to the sender with the name of the router at which it hit zero.

TTL is used to prevent infinite loops on a network. By hitting zero and sending back an error to the sender, transmission can attempt to take a differing path the next time a packet is sent.

*NB: This is actually how traceroute works. By setting the TTL to 0, 1, 2, ... until the destination is hit its able to 'trace' the routing*

## Latency vs Bandwidth

**Latency** is the time it takes data to travel. This is non-dependent on size, but generally dependent upon the number of hops and the distance of those hops. So generally, a New Yorker getting a response from a server in Nanjing would take longer than a server located in Florida.

**Bandwidth** is the amount of data that can be transmitted on a network. It can be thought of as the width of the internet tube that is transmitting data. In this case a two people living in the same building make a request to amazon.com, but one has DSL and the other has gigabit internet, the gigabit connection will receive a largish response quicker. This is because they can be sent more packets per second.

## Middleboxes

**Firewalls** are software defined middleboxes installed on the server. These are very common, but can provide problems for webapps reliant on third party connections that may be blocked.

## Web Proxies

A **web proxy** serves as a router for HTTP. This acts as an intermediary between the client and server. Handling requests and making them for the client, then sending the response back to the client. As a middleware they provide extra features like caching and load balancing. They can also send requests to duplicate servers to spread heavy loads across multiple servers.
