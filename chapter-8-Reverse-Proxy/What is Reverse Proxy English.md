# What is Reverse Proxy ? English

In this next section of the course, we're going to be talking about how we can utilize NGINX as a reverse proxy. But before we begin, I do want to define what a reverse proxy is and what makes it different from a proxy.

First, let's take a look at what a proxy server is. A proxy server sits between our clients and the internet. This means that our clients, such as workstations within our organization, route their traffic through a proxy server before it reaches the internet. This is often used inside organizations as an intermediate layer for monitoring web traffic.

A reverse proxy, on the other hand, repositions where the internet is in that directional flow. A reverse proxy sits between traffic coming from the internet and our servers. It intercepts random requests coming from the internet. We can use this as a layer for load balancing, ensuring we know which application is receiving the traffic, and also to serve static content from a cache, which prevents the need to reach the end server.

Keeping this definition in mind, as we move forward working with reverse proxies, we'll see how we can utilize them to improve the performance of our backend servers.