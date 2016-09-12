# Tools

## Installing

All tools listed below can be installed via `sudo apt-get install <name of tool>` from any linux based console (includes Mac OS)

## netcat (nc)

Utility for TCP, UDP or UNIX sockets. Can read and write to network connections.

```bash
# listens to localhost port 12345
nc -l 12345

# create host at port 12345
nc localhost 12345

# write (send string) to google.com that mimics HTTP get request
# returns response
printf 'GET / HTTP/1.1\r\nHost: google.com\r\n\r\n' | nc google.com 80

```

## tcpdump

Packet analyzer allows the user to display TCP/IP and other packets being transmitted or received over a network. Good when used with nc (in another console) to invoke network traffic.

```bash
# listen to traffic between local machine and google.com
sudo tcpdump -n host google.com

# ... in another console
# invoke traffic
printf 'GET / HTTP/1.1\r\nHost: google.com\r\n\r\n' | nc google.com 80
```

## traceroute

Tracks the hops packets make between client and server.

```bash
# trace hops to google.com server
traceroute google.com
```

## ping

Utility to determine the reachability of a machine on the internet. Sends an 'ICMP echo request' over UDP to the machine, which responds with an 'ICMP echo response'. `-c` followed by a number, n, will ping n times, rather than infinitely continuing.

```bash
ping google.com
```
