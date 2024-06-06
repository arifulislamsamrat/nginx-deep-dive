# Reverse Proxy with proxy_pass ( English )

In this video, we're going to continue working with the Node.js applications that we set up in the previous video, and look at how we can proxy to these servers. This is a photo application, and we'll set this up using the subdomain [photos.example.com](http://photos.example.com/). The main module we'll focus on today is the `http_proxy_module`, along with two companion modules for `fastcgi` and `uwsgi`. These modules have very similar directives, so once we understand one, the others will be easier to work with.

### Reverse Proxies Overview

Previously, we discussed using reverse proxies for load balancing and caching. Now, we'll dive deeper into setting up NGINX to:

- Serve static content efficiently.
- Route specific requests to our application for dynamic content generation or database interactions.

### Initial Proxy Server Setup

1. **Create a Configuration File**:
We'll create a file inside `/etc/nginx/conf.d` named `photos.example.com.conf` with the standard server block listening on port 80. You can set up SSL with a self-signed certificate, but for simplicity, we'll skip that for now.
2. **Define the Server Block**:
The server name will be `photos.example.com`. We haven't set a root directory because we're serving a dynamic web application, not static files. Our web client runs on port 3000.
3. **Proxy Pass Directive**:
We'll use `proxy_pass` to route traffic to `http://127.0.0.1:3000`.

### Additional Proxy Configurations

To handle client IPs and other headers correctly, we'll add the following configurations:

- `proxy_http_version 1.1`
- `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for`
- `proxy_set_header X-Real-IP $remote_addr`
- For WebSockets:
    - `proxy_set_header Upgrade $http_upgrade`
    - `proxy_set_header Connection "upgrade"`

### Reload NGINX and Test

Reload NGINX and test the configuration by accessing `photos.example.com` in a browser. If you encounter permission errors (often due to SELinux), use `setsebool -P httpd_can_network_connect 1`.

### Serving Static Content with NGINX

To serve static content, we'll create a symbolic link from our application's public directory to `/var/www/photos.example.com`. Then, we'll add a new location block in our configuration file to match static file types (e.g., CSS, JS, images) and set the root to `/var/www/photos.example.com`.

### Handling Large File Uploads

If you encounter a "413 Request Entity Too Large" error, set the `client_max_body_size` directive to a higher value (e.g., `5m`).

By understanding the basic setup and configurations for using NGINX as a reverse proxy for Node.js applications, you can now handle more complex scenarios and optimize your web server performance.