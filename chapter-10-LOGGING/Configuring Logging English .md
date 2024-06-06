# Configuring Logging English

In this video, we're going into  deeper into logging with NGINX. We will cover how access logging works, how to customize log formats, and how error logging functions. To start, we'll look into the `/etc/nginx/nginx.conf` file, which contains directives related to logging. By understanding these directives, we can better manage and utilize logs for monitoring and troubleshooting.

### Access Logging and Log Formats

Access logging in NGINX is managed through the HTTP log module, which includes directives like `log_format` and `access_log`.

1. **Log Format**:
    - The `log_format` directive allows us to create named formats for access logs.
    - For example:
        
        ```bash
        log_format compression '$remote_addr - $remote_user [$time_local] '
                               '"$request" $status $body_bytes_sent '
                               '"$http_referer" "$http_user_agent" "$gzip_ratio"';
        ```
        
    - In this example, `compression` is the name of the format, and the string specifies the log entry format.
2. **Using Access Log with Log Format**:
    - To apply a log format, use the `access_log` directive:
        
        ```bash
        access_log /var/log/nginx/access.log compression;
        ```
        
    - If you see the `combined` format in other configurations, know that it is a pre-defined format in NGINX, similar to Apache's log format.

### Error Logging

Error logging in NGINX is simpler than access logging. It involves specifying a file and a logging level:

1. **Error Log Directive**:
    - The directive for error logging looks like this:
        
        ```bash
        error_log /var/log/nginx/error.log warn;
        ```
        
    - The logging level can range from `debug` to `emerge`, in order of severity.
2. **Logging Levels**:
    - `debug`, `info`, `notice`, `warn`, `error`, `crit`, `alert`, `emerge`.
    - Choosing a logging level determines which log entries are recorded. For example, setting it to `error` will record `error` and all more severe entries (`crit`, `alert`, `emerge`).

### Defining a Custom Log Format

To make logs more informative, you can define your custom log format:

1. **Create a Custom Log Format**:
    - In the `nginx.conf` file, define the custom log format:
        
        ```bash
        log_format vhost '$host $remote_addr - $remote_user [$time_local] '
                         '"$request" $status $body_bytes_sent '
                         '"$http_referer" "$http_user_agent"';
        ```
        
2. **Apply the Custom Log Format**:
    - Use the custom format in your server configuration:
        
        ```bash
        access_log /var/log/nginx/access.log vhost;
        
        ```
        
3. **Testing the Configuration**:
    - Reload NGINX to apply the changes and test by making requests:
        
        ```bash
        systemctl reload nginx
        curl -H "Host: notes.example.com" <http://localhost/>
        ```
        
4. **Verify the Logs**:
    - Check the access log to see entries in the new format:
        
        ```bash
        less /var/log/nginx/access.log
        ```
        

### Using Syslog for Logging

In many deployments, logs are sent to a central logging server using syslog:

1. **Configure Syslog for Access Logs**:
    - Change the `access_log` directive to use syslog:
        
        ```bash
        access_log syslog:server=unix:/dev/log vhost;
        ```
        
2. **Syslog Parameters**:
    - Additional parameters can be passed to syslog using a key-value syntax separated by commas.
3. **Viewing Syslog Entries**:
    - On CentOS, check syslog entries in `/var/log/messages`:
        
        ```bash
        less /var/log/messages
        ```
        

So,we have explored NGINX logging in depth. We covered how to define custom log formats, how to configure access and error logs, and how to use syslog for logging. Understanding and customizing these logging configurations can help you effectively monitor and troubleshoot your NGINX servers.