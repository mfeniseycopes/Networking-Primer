# HTTP Lifecycle Overview

Just as a review, here are the details of the HTTP request lifecyle. This doesn't go into networking too much, but is helpful to understand the steps that lead to network transmissions.

## What happens when you type a url in a browser?

![](/images/http-sequence.png)

### URL breakdown

The URL is not merely where we want, but also what we want.

* `http://` - protocol in which data is sent
* `google.com` - domain name which has a DNS mapping to the server IP
* `/search` - route, which describes to the server what resource you are interacting with
* `?id=15&name=meagan` - query string, which is used to pass additional data to the server in a GET request. `?` denotes the beginning of the query string, with each key/value pair joined by a `=` and listed by separate `&`

The domain name is linked to a specific IP, which is where the server is located.

### Making the connection

1. The browser checks to see if the IP for that domain is already cached
2. If it is not cached, the the browser asks for a DNS (Domain Name Server) lookup for the IP
3. When the IP is returned, the browser opens a TCP connection with the server
 * A TCP (Transmission Control Protocol) connection ensures that all data is successfully transmitted and received
4. The browser then makes a HTTP request to the server
5. The server receives the request, builds, sends response via TCP connection
  * response may contain, javascript, HTML, CSS, static images
6. The browser receives the response, depending on response code will redirect, display an error or render the page
7. If possible, the browser will cache the response to use again later
