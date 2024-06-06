# Workers & Connections English

### Adjusting Server Performance with NGINX Configuration

we're going to talk about how we can adjust the performance of our server by modifying `worker_processes`, `worker_connections`, and then taking a look at keepalives. To get started, let's open up the `nginx.conf` that holds our main configuration for NGINX itself, and then up at the top, we're going to have two things. We're going to have `worker_processes`, which is set to one currently. And then within our `events` context, we're going to have `worker_connections`, which is set to 1024. Before we start modifying these, let's talk about what these do.

NGINX starts as a master process, and it can have multiple `worker_processes` underneath it. The ideal value for `worker_processes` is the number of CPU cores you have. You want that many worker processes for a server dedicated to running NGINX.

`worker_connections` is the number of connections each worker process can handle at a given time. To determine the total number of connections your server can handle, you multiply `worker_processes` by `worker_connections`. Finding the balance of `worker_processes` is easy, but determining the proper number of `worker_connections` requires some trial and error.

### Configuring `worker_processes`

To adjust `worker_processes`, you can set a specific number or use `auto`. The `auto` setting will detect the number of CPU cores and set `worker_processes` accordingly. This ensures NGINX uses all available CPU cores for optimal performance. Let's modify `worker_processes` to `auto`:

```bash
worker_processes auto;
```

### Configuring `worker_connections`

Now let's focus on `worker_connections`. The default value is often 512, but it’s set to 1024 in our configuration. These numbers are low for modern servers. NGINX was designed to handle 10,000 requests per second over a decade ago, so we can safely set this higher. We'll double the `worker_connections` to 2048 to prevent dropping connections during traffic spikes:

```bash
worker_connections 2048;
```

After making these changes, save the file and check the syntax to ensure there are no errors:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

If you encounter warnings about exceeding the number of open files, you can set the `worker_rlimit_nofile` directive to match the system's hard limit for open files:

```bash
worker_rlimit_nofile 4096;
```

### Configuring Keepalives

Keepalives allow a single TCP connection to handle multiple requests, reducing the overhead of establishing new connections. We configure this using `keepalive_timeout` and `keepalive_requests`. The default `keepalive_timeout` is 75 seconds. Adjust this based on client behavior and the typical duration of activity on your site:

```
keepalive_timeout 65;

```

The `keepalive_requests` directive specifies the maximum number of requests that a single keepalive connection can handle, with a default of 100. If a client makes many requests quickly, increase this value to avoid additional TCP handshakes:

```bash
keepalive_requests 100;
```

### Using Keepalives with Upstream Servers

For connections between NGINX and upstream servers, use the `upstream` module’s `keepalive` directive to maintain a pool of cached connections. Set the HTTP version to at least 1.1 and clear or upgrade the connection header:

```bash
upstream backend {
    server backend1.example.com;
    keepalive 32;
}
```

```bash
location / {
    proxy_http_version 1.1;
    proxy_set_header Connection "";
    proxy_pass <http://backend>;
}
```

So, We’ve covered how to adjust `worker_processes`, `worker_connections`, and keepalives to improve NGINX performance. The key points are:

- Set `worker_processes` to `auto` to use all CPU cores.
- Increase `worker_connections` to handle more simultaneous connections.
- Configure keepalives to maintain connections longer, reducing the overhead of new connections.
- Use keepalives with upstream servers to maintain a pool of cached connections, improving efficiency.

By fine-tuning these settings, you can optimize your NGINX server to handle higher traffic efficiently without dropping connections.