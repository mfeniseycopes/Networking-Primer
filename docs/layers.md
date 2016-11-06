# The Internet Protocol Stack

The way we use the web is not simply opening a direct connection between two devices and sending a bunch of files. For devices of differing hardware and architecture to communicate with one another they have to follow certain standards or *protocols* which, in essence, allow them to speak the same language.

When browsing the internet we typically type a url in our browser and are displayed a (hopefully) pretty website. But there is much more than just receiving a webpage.

### Typical Web Application Protocol Stack
```
HTTP (defines the structure of data)
> TCP (defines how the data is transmitted)
  > IP (defines how a connection is made)
    > Hardware (must support IP model w/ appropriate architecture)
```

Each protocol used when browsing the web is based upon a lower protocol, with the base layer protocol being the hardware interface (wifi, ethernet, lo). Each stacked layer encapsulates and abstracts the layer below it. This means that developers using HTTP don't need to know the details of the TCP protocol but can 'just use it' via the HTTP interface.
