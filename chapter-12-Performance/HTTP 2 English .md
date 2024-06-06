# HTTP/2 English

we're going to take a look at HTTP2, or H2, as a way to boost the performance of our servers. Before we dive into how to utilize and enable it for our servers, let's discuss the requirements for using HTTP2 with NGINX and the benefits it offers.

### Benefits of HTTP2

1. **Improved Encryption**: HTTP2 offers better encryption compared to HTTP 1.1.
2. **Multiplexed Streams**: It allows multiple resources to be requested using a single TCP connection simultaneously, solving the issue of connection limits. For instance, you might encounter a limit on the number of TCP connections you can have open to the same server at a time. With HTTP2, multiple requests can be made simultaneously to the same domain using a single connection.
3. **Server Push**: Server push allows the server to send resources to the client before the client explicitly requests them. For example, when the client requests an HTML file, the server can push the associated CSS and JavaScript files.
4. **Header Compression**: HTTP2 uses header compression (HPACK) to reduce redundant header data, making data transmission more efficient.

### Enabling HTTP2 in NGINX

To enable HTTP2, you need to serve your content over SSL and set the HTTP2 attribute in your `listen` directive. Here's how you can do it:

1. Open your `nginx.conf` or appropriate server configuration file.
2. Locate the `listen 443 ssl` directive and add `http2` to it:
    
    ```
    listen 443 ssl http2;
    
    ```
    
3. Save the configuration file.
4. Test your configuration for syntax errors:
    
    ```
    sudo nginx -t
    
    ```
    
5. Reload NGINX to apply the changes:
    
    ```
    sudo systemctl reload nginx
    
    ```
    

After these steps, check your website to ensure HTTP2 is enabled. You can verify this by inspecting the network requests in your browser's developer tools. The protocol version should be listed as HTTP2.

By following these steps, you can easily enable HTTP2 and enjoy its benefits, which include improved website performance and efficiency.