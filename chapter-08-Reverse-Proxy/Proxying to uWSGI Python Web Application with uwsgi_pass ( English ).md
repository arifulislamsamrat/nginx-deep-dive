# Proxying to uWSGI Python Web Application with uwsgi_pass ( English )

In this session, we will set up proxying to a uWSGI application using the `nginx_http_uwsgi_module`. This setup is similar to FastCGI and proxy modules, with some common interfaces and directives. We will focus on using the `uwsgi_pass` directive and handling static assets with NGINX.

### Step-by-Step Setup

1. **Create NGINX Configuration File**:
    - Navigate to the NGINX configuration directory:
        
        ```bash
        cd /etc/nginx/conf.d
        ```
        
    - Create a configuration file for your domain:
        
        ```bash
        nano notes.example.com.conf
        ```
        
2. **Edit Configuration File**:
    - Add the following configuration to the file:
        
        ```
        server {
            listen 80;
            server_name notes.example.com;
        
            # Serve static files
            location /static/ {
                alias /var/www/notes.example.com/static/;
            }
        
            # Proxy requests to uWSGI
            location / {
                include uwsgi_params;
                uwsgi_pass unix:/var/run/uwsgi/notes.sock;
            }
        }
        ```
        
3. **Verify uwsgi_params File**:
    - Ensure the `uwsgi_params` file is present and correctly configured. This file maps NGINX directives to uWSGI parameters.
4. **Reload NGINX**:
    - Check the NGINX configuration for errors and reload the service:
        
        ```bash
        nginx -t
        systemctl reload nginx
        ```
        
5. **Update /etc/hosts File**:
    - Modify the `/etc/hosts` file to map the domain to your server's IP address:
        
        ```bash
        nano /etc/hosts
        ```
        
    - Add the following line:
        
        ```
        127.0.0.1 notes.example.com
        ```
        
6. **Address SELinux Issues**:
    - If you encounter a 502 Bad Gateway error due to SELinux restrictions, create an SELinux policy:
        
        ```bash
        grep nginx /var/log/audit/audit.log | audit2allow -m nginx > nginx.te
        checkmodule -M -m -o nginx.mod nginx.te
        semodule_package -o nginx.pp -m nginx.mod
        semodule -i nginx.pp
        semodule --enable nginx
        ```
        
    - Restore SELinux contexts:
        
        ```bash
        restorecon -Rv /var/www/notes.example.com
        restorecon -Rv /var/run/uwsgi
        ```
        
7. **Test the Application**:
    - Open a web browser and navigate to `http://notes.example.com`. Ensure the application loads and serves static assets correctly.
    - Verify that dynamic content, such as user-generated notes, is functioning as expected.

### Additional Details

- **Static Assets**:
    - The `make static` command places static files in `/var/www/notes.example.com/static`. This directory is used by NGINX to serve static content.
- **uWSGI Socket**:
    - The uWSGI socket file is located at `/var/run/uwsgi/notes.sock`. This socket is specified in both the NGINX configuration and the uWSGI service file.
- **Reverse Proxy Benefits**:
    - A reverse proxy server sits between internet traffic and your backend application servers. It routes traffic to the appropriate socket or port, serves static content, and can be configured for caching.

###