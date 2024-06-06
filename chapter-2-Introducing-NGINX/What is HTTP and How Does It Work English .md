# What is HTTP and How Does It Work? English

We're going to talk about HTTP for the moment because this is going to be the main protocol used throughout our time working with NGINX. I want everybody to have a little bit of background as to what HTTP is and how it works before we get started.

### What is HTTP?

HTTP stands for Hypertext Transfer Protocol, and it's really the language of the internet. It's what we run into anytime we're making web requests in our browser or using tools like curl. HTTP is the application layer protocol built on top of TCP/IP. TCP/IP is the transport layer protocol for shipping packets and data over the internet. HTTP is used to tell the server or service on the other side of a request what we want it to do. This is the language that two programs speak to display their intent. As a protocol, HTTP defines how information is sent over the internet, allowing the implementation of servers and clients.

### HTTP vs. HTTPS

HTTPS stands for Hypertext Transfer Protocol Secure. It is essentially HTTP with an added layer for TLS (Transport Layer Security) or SSL (Secure Sockets Layer). This is where you get HTTPS and the green lock in your browser, indicating that the content is encrypted between the client and server.

### Structure of a URL

One of the main ways we interact with HTTP is by utilizing a URL. Breaking down a URL into its components:

- **Protocol**: `http://` or `https://`
- **Host Name**: Domain or IP address
- **Port**: Optional; defaults to 80 for HTTP and 443 for HTTPS
- **Path**: Route to the resource
- **File Name**: Specific resource being requested

For example, `http://example.com:80/path/to/resource`:

- `http` is the protocol.
- `example.com` is the host name.
- `80` is the port.
- `/path/to/resource` is the path.
- `resource` is the file name.

### HTTP Requests and Responses

An HTTP request includes:

- **Verb**: Method, such as GET or POST.
- **URI**: Path of the resource.
- **Headers**: Key-value pairs, such as `Host: blog.example.com` or `User-Agent: curl`.

The request ends with a blank line indicating the end of the headers.

Example of an HTTP GET request:

```
GET / HTTP/1.1
Host: blog.example.com
User-Agent: curl
Accept: */*

```

An HTTP response includes:

- **Status Line**: Protocol version and status code, such as `HTTP/1.1 200 OK`.
- **Headers**: Information like server type, date, and content type.
- **Body**: Actual content, such as an HTML page.

Example of an HTTP response:

```
HTTP/1.1 200 OK
Server: nginx/1.18.0
Date: Thu, 01 Jan 1970 00:00:00 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 612
Connection: keep-alive

<html>
<head><title>Example</title></head>
<body>...</body>
</html>

```

This understanding of HTTP is crucial for effectively using NGINX, as it allows you to configure and troubleshoot web server interactions more effectively.