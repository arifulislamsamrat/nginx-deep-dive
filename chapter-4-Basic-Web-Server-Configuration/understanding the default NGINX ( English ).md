# understanding the default NGINX ( English )

In this video, we dive into the fundamentals of configuring NGINX. The main focus is on tweaking configuration options to make NGINX work according to our requirements. We start by examining the default configuration that comes with NGINX, which includes the server configuration and the initial virtual hosts. We'll make some modifications to this default virtual host and explore the default server settings.

Throughout this course, we'll use the root user or sudo because administrative privileges are necessary for editing NGINX's configuration files and interacting with systemd. We'll switch to the root user using **`sudo su`** to access and modify the files in the **`/etc/nginx`** directory, where most of NGINX's configuration files are located. The primary configuration file is **`nginx.conf`**, while virtual hosts are configured in the **`conf.d`** directory.

Understanding the structure of **`nginx.conf`** is crucial. This file contains various directives and contexts. Directives like **`user`** and **`worker_processes`** operate at the main context level, while others, such as **`worker_connections`**, are within specific contexts like **`events`**. The **`events`** context is mandatory, configuring how connections are handled by worker processes.

The **`http`** context, which we'll explore in detail, defines the HTTP handling configuration of NGINX. This context allows us to include other configuration files, making it easier to manage settings like MIME types, which determine the content type returned by the server based on file extensions.

Key directives include:

- **`error_log`**: Specifies the log file and log level.
- **`pid`**: Indicates the file holding the process identifier.
- **`worker_connections`**: Sets the maximum number of connections per worker process.

Additional configurations such as **`sendfile`** for performance optimization, **`keepalive_timeout`** for connection management, and **`gzip`** for compression will be discussed in future videos. Comments in the configuration file are marked with a **`#`** and are useful for documentation.

Finally, the **`include`** directive in **`nginx.conf`** allows the inclusion of all **`.conf`** files in the **`conf.d`** directory, enabling the logical separation of virtual host configurations. This modular approach means we generally won't need to modify **`nginx.conf`** directly, focusing instead on files in the **`conf.d`** directory.

In the next video, we'll create a new **`default.conf`** from scratch, learning the essentials of building a simple HTTP server configuration for a virtual host.

What directive will you use to include functionality from a dynamic module?

None of these choices.

The load_module directive.

The module directive.

The include directive.