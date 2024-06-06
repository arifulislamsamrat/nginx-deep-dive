# Examining Load Balancing Methods English

In this video, we will go  deeper into load balancing within NGINX, focusing on how we can modify the load balancing methods. By knowing  various directives and configurations, we can know  how traffic is distributed across servers in our application. We will explore these concepts through practical examples and documentation references, applying them to our existing `photos.example.com` upstream directive.

### NGINX Load Balancing Methods

NGINX provides several methods for load balancing, each suited for different scenarios:

1. **Default (Round Robin)**
    - This is the default method if no specific load balancing method is defined.
    - It distributes incoming traffic evenly across all servers.
2. **Hash**
    - This method routes traffic based on a specified key, such as `$request_uri`.
    - Ensures repeat requests are directed to the same server, which can help with caching.
    - **Risk**: Can lead to uneven traffic distribution if one URI gets most of the traffic.
    - **Use Case**: Direct admin area traffic to a specific server to prevent it from affecting general user traffic.
3. **IP Hash**
    - Routes traffic based on the client's IP address (first three octets of IPv4).
    - Ensures clients from the same network are routed to the same server.
    - **Risk**: Potentially overloads a server if many clients are from the same network.
4. **Least Connections (least_conn)**
    - Routes traffic to the server with the fewest active connections.
    - Balances the load based on real-time server activity.
    - Effective in environments with varying request lengths.
5. **NGINX Plus Exclusive: Least Time**
    - Routes traffic based on average response time.
    - Optimizes performance by sending traffic to servers responding faster.

### Configuring Load Balancing Methods

To change the load balancing method, modify the upstream block in your NGINX configuration:

- **Example Configuration for IP Hash:**
    
    ```
    upstream photos {
        ip_hash;  # Using IP hash method
        server server1;
        server server2;
        server server3;
    }
    ```
    
- **Hash Method with Argument:**
    
    ```
    upstream photos {
        hash $request_uri;  # Using request URI as key
        server server1;
        server server2;
        server server3;
    }
    ```
    

**Best Practice**: Place the load balancing directive at the top of the upstream block to ensure clarity and proper function.

### Server Parameters for Traffic Management

NGINX allows further customization of load balancing through various server parameters:

- **Weight**
    - Sets priority for traffic distribution.
    - Higher weight means higher priority.
    - **Example**: `server server1 weight=5;`
- **Max Connections (max_conns)**
    - Limits the maximum number of connections to a server.
    - **Example**: `server server1 max_conns=100;`
- **Backup**
    - Designates a server as a backup, used only when primary servers fail.
    - **Example**: `server server3 backup;`
- **Down**
    - Marks a server as unavailable, useful for maintenance.
    - **Example**: `server server2 down;`

### Passive Health Checking

Passive health checking helps monitor server health by tracking failures:

- **Max Fails (max_fails)**
    - Specifies the number of failed requests before marking a server as unavailable.
    - **Example**: `max_fails=3;`
- **Fail Timeout (fail_timeout)**
    - Sets the period to count failures and the time to mark the server as unavailable.
    - **Example**: `fail_timeout=20s;`

These settings allow NGINX to temporarily remove a server from the pool if it fails, providing it time to recover.

NGINX supports multiple load balancing methods: round robin (default), ip_hash, hash, and least_conn. These methods can be fine-tuned using server parameters like weight, max_conns, backup, and down. Additionally, passive health checking with max_fails and fail_timeout enhances reliability by ensuring servers are only used when healthy. By understanding and applying these methods and configurations, you can optimize traffic distribution and server performance within your NGINX setup.