# what is NGINX English

### History and Background

Before we dive into demonstrations and hands-on work with NGINX, it's important to cover some history and background about NGINX. NGINX is not an incredibly old project; it has existed since 2004 when it was open-sourced by its creator, Igor Sysoev. NGINX was originally written to solve a specific problem, known as the C10K problem, which refers to handling 10,000 concurrent connections.

Igor Sysoev faced various issues using other web servers and aimed to build a web server capable of handling high stress. The architecture of NGINX is designed to solve this problem efficiently. After being open-sourced, a community began to form around NGINX, leading to the creation of a commercial wing, NGINX, Inc, which offers a commercial product called NGINX Plus.

This course will not cover NGINX Plus in detail. We will briefly touch on some differences and additional features available with an NGINX Plus license. Occasionally, in the documentation, you may find features that seem useful but are only part of the commercial product.

### Features of NGINX

NGINX is known for being a high-performance web server. This is one of the main attractions for people interested in NGINX. Something you might not know is that 50% of the top 1000 websites on the internet use NGINX. Few pieces of software are as thoroughly tested and proven as NGINX, and its configuration is relatively simple, which has contributed to its widespread adoption.

NGINX is frequently used as a reverse proxy. It can handle SSL/TLS termination so that backend applications do not need to manage SSL handshakes. Additionally, NGINX can cache content and compress it, reducing the number of bytes sent over the network to the end client.

### Load Balancing

Another major use case for NGINX is as a load balancer. This functionality goes hand-in-hand with being a reverse proxy. A load balancer can take requests for a specific server or domain and route them to the appropriate backend server using methods like round-robin or load-based routing.

Throughout this course, we will explore various practical use cases for NGINX. However, having a background understanding of NGINX before we start is crucial to fully appreciate its capabilities and benefits.