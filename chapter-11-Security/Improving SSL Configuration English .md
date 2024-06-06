# Improving SSL Configuration English

In this section, we will discuss security, specifically diving deeper into SSL and TLS configuration for our servers. We will explore how to configure SSL and TLS settings effectively.

### Initial Setup

So far, we have done the bare minimum to get traffic over HTTPS using SSL. In this video, we will modify our default SSL setup to use more specific configuration options to better understand what the SSL module provides.

### SSL Configuration

We will learn how the SSL module works and follow best practices.

### SSL Sessions

1. **Session Timeout**:
    - Set the session timeout to one day.
    - This helps avoid renewing the SSL handshake every time a client connects.
2. **Caching**:
    - Enable SSL session caching and set the shared cache size to 50 MB.
    - Shared cache means all NGINX worker processes will use the same cache.
3. **Disabling Session Tickets**:
    - Turn off session tickets to keep SSL sessions stored on the server instead of the client.

### SSL Protocols and Ciphers

1. **SSL Protocols**:
    - Use only TLS 1.2, supporting modern browsers.
2. **SSL Ciphers**:
    - Use the cipher suite suggested by Mozilla.
    - Enable `ssl_prefer_server_ciphers` to prefer server-specified ciphers over client-preferred ones.

### HSTS and OCSP Stapling

1. **HSTS (HTTP Strict-Transport-Security)**:
    - Enable HSTS to ensure clients interact with the server only over HTTPS.
    - Use the `Strict-Transport-Security` header and set it for 6 months.
2. **OCSP Stapling**:
    - Enable OCSP Stapling to pre-fetch certificate validity and serve it to clients.

we discussed SSL and TLS configuration in detail, learned best practices, and used guidance from experts like Mozilla. In managing SSL and TLS security, it's best to follow expert recommendations.

### Implementation Steps

1. Navigate to the `/etc/nginx` directory.
2. Open the `conf.d/default.conf` file.
3. Configure the SSL settings:
    - `ssl_session_timeout 1d;`
    - `ssl_session_cache shared:SSL:50m;`
    - `ssl_session_tickets off;`
    - `ssl_protocols TLSv1.2;`
    - `ssl_ciphers` and `ssl_prefer_server_ciphers on;`
    - `add_header Strict-Transport-Security "max-age=15768000";`
    - `ssl_stapling on;`
4. Save the changes and reload NGINX.

These configurations will help enhance your server security and follow the best practices for SSL and TLS.