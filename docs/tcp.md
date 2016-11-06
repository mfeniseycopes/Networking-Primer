# Transmission Control Protocol (TCP)

TCP (Transmission Control Protocol) is an agreed upon system by which most internet data is transmitted and received. In the Internet Protocol Stack, it sits between HTTP (which describes the data) and IP (which describes the connection).

## What happens at the TCP layer when an HTTP request is made?

Data sent across a TCP connection is not sent in one large file, but is broken into smaller length packets. These packets are numbered into a sequence and sent using the Internet Protocol to the recipient. Upon receipt of each package, the recipient sends an ACK (or acknowledgement packet) with the sequence number to 'acknowledge' that they received it. This is necessary because packets are not guaranteed successful delivery and the sender will continue to send the packets until they know for certain they have been received.

For example, in a typical GET request the initiator sends the GET request packet to the responder. The responder ACKs the request, then sends the data packet(s), each of which has a sequence number. Each data packet is ACKed by the initiator as it comes in. This tells the responder that the packet is received and they can cease sending it.

### TCP Sequence Diagram

![TCP Sequence Diagram](/images/tcp-sequence.png "TCP Sequence Diagram")

## TCP Flags

Each packet can have single bit flags set to identify certain properties or intents of the package.

* SYN (synchronize) [S] — This packet is opening a new TCP session and contains a new initial sequence number.
* FIN (finish) [F] — This packet is used to close a TCP session normally. The sender is saying that they are finished sending, but they can still receive data from the other endpoint.
* PSH (push) [P] — This packet is the end of a chunk of application data, such as an HTTP request.
* RST (reset) [R] — This packet is a TCP error message; the sender has a problem and wants to reset (abandon) the session.
* ACK (acknowledge) [.] — This packet acknowledges that its sender has received data from the other endpoint. Almost every packet except the first SYN will have the ACK flag set.
* URG (urgent) [U] — This packet contains data that needs to be delivered to the application out-of-order. Not used in HTTP or most other current applications.

## 3-way handshake

The 3-way handshake is how a connection is initialized between a client and server. The client sends an initial packet with the SYN flag set and an initial sequence number. Then the server responds with a packet having a SYN flag, the ACK to the initial sequence number and setting its own initial sequence number. The client then responds with it's next sequence number, ACKing the server's response.

This sets a baseline for the sequence numbers and both client and server acknowledge that they are connected and will send and receive data to each other.

## 4-way breakup

When one side has finished sending data it will send a FIN flagged packet, telling the other it is done. The other will acknowledge, but continue to send data until it also finishes. At this point it will also send a FIN flagged packet and the side that finished first will acknowledge. At this point both have finished and they will break their connection.

## TCP Congestion

Frequently, fast networks will be sending packets to one another through a slower network. This can easily cause bottlenecks or packet loss. To prevent this TCP slows down the packet send rate each time it does not receive an ACK response. This can be seen by attempting to connect to a closed port.

After enough sends and no ACK responses, the connection will timeout.

Try connecting to `www.udactity.com:12345`.

```bash
# In 1st console
tcpdump -n port 12345

# In 2nd console
nc www.udacity.com 12345
```
