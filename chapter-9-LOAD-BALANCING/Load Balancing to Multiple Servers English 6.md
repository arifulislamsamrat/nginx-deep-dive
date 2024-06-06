# Load Balancing to Multiple Servers  English

we will  explore how to use NGINX as a reverse proxy to load balance traffic across multiple application servers. This can be useful whether the servers are on the same machine or distributed across different machines. By balancing the traffic, you can improve the scalability and reliability of your application.

**Objective:**

- Utilize NGINX to distribute traffic among multiple application servers.
- Demonstrate the process with instances running on the same physical server.

**Setup:**

1. **Create Multiple Instances:**
    - Duplicate the existing application to create multiple instances.
    - For example, use the `cp` command to create copies of the web client application:
        
        ```
        cp -r web-client web-client2
        cp -r web-client web-client3
        
        ```
        
2. **Modify Ports:**
    - Change the port numbers for each instance to avoid conflicts.
    - For `web-client2`, set the port to 3100:
        
        ```
        environment PORT=3100
        
        ```
        
    - For `web-client3`, set the port to 3101:
        
        ```
        environment PORT=3101
        
        ```
        
3. **Start and Enable Services:**
    - Start the newly created services and ensure they are running correctly.

**NGINX Configuration:**

1. **Define Upstream Servers:**
    - Use the `upstream` directive in NGINX to create a pool of servers:
        
        ```
        upstream photos {
            server 127.0.0.1:3000;
            server 127.0.0.1:3100;
            server 127.0.0.1:3101;
        }
        
        ```
        
2. **Modify Proxy Pass:**
    - Update the `proxy_pass` directive to use the upstream server group:
        
        ```
        server {
            location / {
                proxy_pass <http://photos>;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
            }
        }
        
        ```
        
3. **Reload NGINX:**
    - Apply the new configuration by reloading NGINX:
        
        ```
        systemctl reload nginx
        
        ```
        

**Testing Load Balancing:**

- Use `curl` to send multiple requests and observe how they are distributed:
    
    ```
    curl -H "Host: photos.example.com" <http://localhost>
    curl -H "Host: photos.example.com" <http://localhost>
    
    ```
    

**Observations:**

- Check the logs or status of each web client instance to see how requests are distributed.
- NGINX uses a round-robin approach by default, distributing requests sequentially among the servers.

This setup allows NGINX to distribute traffic among multiple servers, improving scalability. Further customization of load balancing methods (e.g., least connections, IP hash) can be done based on specific needs.