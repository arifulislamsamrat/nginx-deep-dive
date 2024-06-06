# Overview Of NGINX module ( English )

We have been using various modules, but now we will understand where they come from, how to get more, and how to know which modules are available.

### Understanding Modules Directory

1. **Modules Directory Location:**
    - We start in the `/etc/nginx` directory.
    - By running `ls`, we see a `modules` directory.
    - Using `ls -al`, we discover that `modules` is a symlink to `usr/lib64/nginx/modules`.
2. **Dynamic Modules:**
    - The `modules` directory is meant for dynamic modules.
    - Since NGINX version 1.9.11, NGINX supports dynamic modules.
    - Dynamic modules can be added without recompiling NGINX entirely, as long as they are compiled with the same settings as the NGINX binary.

### Adding and Identifying Modules

1. **Checking Available Modules:**
    - Depending on how NGINX is installed, you may have modules compiled in or available as dynamic modules.
    - To see the configuration parameters used to build NGINX, use the command `nginx -V`.
2. **Using `nginx -V`:**
    - Running `nginx -V` gives detailed configuration arguments, including which modules are available.
    - To list all modules, use:
        
        ```bash
        nginx -V 2>&1 | tr ' ' '\\n' | grep module
        ```
        

### Types of Modules

1. **Module Types:**
    - There are three main types of modules: HTTP, Mail, and Stream.
    - HTTP modules handle web traffic.
    - Mail and Stream modules handle mail and streaming traffic, respectively, but are not enabled by default.
2. **Slimming Down NGINX:**
    - You can create a slimmer version of NGINX by excluding certain modules during compilation using flags like `-without-http` or `-without-mail`.

### Dynamic Modules in Detail

1. **Loading Dynamic Modules:**
    - Dynamic modules can be loaded using the `load_module` directive in the configuration file.
    - These modules typically have a `.so` extension.
2. **Benefits of Dynamic Modules:**
    - Dynamic modules allow you to add or remove functionalities without recompiling NGINX.
    - This means you can reload your configuration and add new functionalities while NGINX is still running.

### Practical Use of Dynamic Modules

In our course, we will focus on using dynamic modules. One notable example is ModSecurity, a web application firewall, which we will set up as a dynamic module in the next video.

By understanding and utilizing NGINX modules effectively, you can enhance the functionality and flexibility of your server without downtime or extensive recompilation efforts.