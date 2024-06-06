# FastCGI Proxy for PHP/WordPress with fastcgi_pass ( English )

### Setting Up Virtual Host Server Configuration for WordPress with NGINX

In this video, we're going to configure a virtual host server for the WordPress application we set up previously. We'll use NGINX as a reverse proxy, communicating via FastCGI over a socket, and employ the `nginx_http_fastcgi_module`.

### FastCGI Module vs. HTTP Proxy Module

The `proxy_pass` and `fastcgi_pass` directives function similarly, but the latter communicates using the FastCGI protocol instead of HTTP. The configuration differences mainly lie in handling parameters instead of headers.

### Configuration Steps

1. **Create Configuration File**:
    
    ```
    vim /etc/nginx/conf.d/blog.example.com.conf
    
    ```
    
    This file will contain a server block listening on port 80 with the server name set to `blog.example.com`.
    
2. **Set Root Directory and Location Blocks**:
    
    ```
    server {
        listen 80;
        server_name blog.example.com;
    
        root /var/www/blog.example.com;
    
        location / {
            try_files $uri $uri/ /index.php?$args;
        }
    
        location ~ \\.php$ {
            fastcgi_index index.php;
            fastcgi_pass unix:/var/run/php-fpm.sock;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
    }
    
    ```
    
3. **Edit `fastcgi_params` File**:
    
    ```
    vim /etc/nginx/fastcgi_params
    
    ```
    
    This file defines parameters for the FastCGI protocol.
    

### Reload Server and Update `/etc/hosts`

1. **Reload NGINX**:
    
    ```
    systemctl reload nginx
    
    ```
    
2. **Update `/etc/hosts` File**:
    
    ```
    sudo vim /etc/hosts
    
    ```
    
    Add `blog.example.com` to access it via the browser.
    

### WordPress Setup and Configuration

1. **Complete WordPress Installation**:
    - Visit `blog.example.com` in your browser.
    - Complete the WordPress setup wizard and create a test blog.
    - Set a username and password.
    - Log in to the admin dashboard.

The main difference between `proxy_pass` and `fastcgi_pass` lies in using the FastCGI protocol instead of HTTP. FastCGI can be more efficient since it bypasses the network stack by communicating directly over a socket.