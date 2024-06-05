# Simple Virtual Host and Serving Static Content ( English )

**Setting Up Virtual Hosts and Configuring NGINX**

In this video, we're going to set up our first virtual host and reconfigure the default virtual host. This will help us understand what happens when we send HTTP traffic to our server.

**Steps:**

1. **Login as Root:**
    - Ensure you're logged in as root to avoid using `sudo` for every command.
2. **Remove Default Configuration:**
    - Delete the `conf.d/default.conf` file. This allows us to create a new configuration from scratch.
    - Execute `systemctl reload nginx` to apply the changes. Check the status with `systemctl status nginx`.
3. **Create New Default Configuration:**
    - Use a text editor like `vim` to create `conf.d/default.conf`.
    - Define a new server block with:
        - `listen 80;` to listen on port 80.
        - `root /usr/share/nginx/html;` to serve files from this directory.
    - Save and quit the editor.
    - Test the configuration with `nginx -t` and reload NGINX with `systemctl reload nginx`.
4. **Verify Configuration:**
    - Use `curl localhost` to check if the default index.html page is served correctly.
    - Explore `/usr/share/nginx/html` to see the files being served.
5. **Set Default Server and Server Name:**
    - Modify `conf.d/default.conf` to explicitly set it as the default server with:
        - `listen 80 default_server;`
        - `server_name _;` to indicate no specific name.
6. **Create Another Virtual Host:**
    - In `conf.d`, create a new file `example.com.conf`.
    - Define a new server block for `example.com` and `www.example.com` with:
        - `listen 80;`
        - `server_name example.com www.example.com;`
        - `root /var/www/example.com/html;`
    - Save and quit the editor.
    - Create the necessary directories and index.html file for the new virtual host:
        - `mkdir -p /var/www/example.com/html`
        - `echo "Hello, Example.com" > /var/www/example.com/html/index.html`
    - Test and reload the configuration with `nginx -t` and `systemctl reload nginx`.
7. **Test the New Virtual Host:**
    - Use `curl -H "Host: example.com" localhost` to test if the new virtual host serves the correct content.
8. **Handling SELinux Issues:**
    - If you encounter a 403 Forbidden error, it may be due to SELinux restrictions.
    - Check the error log: `less /var/log/nginx/error.log`.
    - Use `semanage fcontext` to allow NGINX to serve files from the new directory:
        - `semanage fcontext -a -t httpd_sys_content_t "/var/www(/.*)?"`
    - Apply the changes with `restorecon -Rv /var/www`.

**So,** we have learned how to set up a new default configuration and our first specific virtual host for `example.com` and `www.example.com`. We configured the server to serve files from different directories and handled SELinux-related issues to ensure proper file serving.