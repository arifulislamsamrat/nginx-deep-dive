# Configuring the Host for SSL / TLS / HTTPS ( English )

In the previous video, we generated our own self-signed SSL certificates. In this video, we'll update our default server configuration to handle HTTPS connections. We'll be working with the ssl_module, which provides various settings for SSL, but for now, we'll focus on the simplest SSL configuration for one of our virtual servers.

First, it's important to note that the **`ssl`** directive is outdated and should be used with the **`listen`** directive. The two main directives we'll use are **`ssl_certificate`** and **`ssl_certificate_key`**. The **`ssl_certificate`** directive points to the public certificate file, and the **`ssl_certificate_key`** directive points to the private key file.

Here's a step-by-step guide:

1. **Open the Configuration File**: We'll open the **`conf.d/default.conf`** file. Since we already have a **`listen`** directive for HTTP, we can add another for HTTPS to handle both types of traffic using the same server configuration.
2. **Add Listen Directive for HTTPS**: Add **`listen 443 ssl;`** to specify that the server should listen on port 443 (the default port for HTTPS) and use SSL.
3. **Specify SSL Certificate and Key**: Add the following lines to point to your SSL certificate and private key:
    
    ```bash
    
    ssl_certificate /etc/nginx/ssl/public.pem;
    ssl_certificate_key /etc/nginx/ssl/private.key;
    ```
    
4. **Test Configuration**: Save the configuration file and run **`nginx -t`** to check for any syntax errors.
5. **Reload NGINX**: If there are no errors, reload NGINX using **`systemctl reload nginx`**.
6. **Test HTTPS in Browser**: Open your browser and navigate to **`https://your-server-ip`**. You will see a warning that your connection is not secure because you are using a self-signed certificate. Click on "Advanced" and then "Proceed" (or the equivalent in your browser) to bypass the warning.
7. **Test HTTPS with Curl**: Use curl to test the HTTPS connection. Running **`curl https://localhost`** will show a security warning. To bypass this, use the **`k`** or **`-insecure`** flag: **`curl -k https://localhost`**.

By following these steps, you've configured a basic HTTPS server. Your server now listens on port 443 for HTTPS connections, using the specified SSL certificate and private key. Even though the self-signed certificate will show security warnings, it allows you to test and ensure your SSL setup is functioning correctly.