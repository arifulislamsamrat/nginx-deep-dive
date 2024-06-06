# Use Case: PageSpeed by Google English

we will explore another use case segment where we utilize a specific tool requested by users: Google's PageSpeed module, also known as NGINX PageSpeed. This module is designed to optimize web content, improving performance and reducing load times. While a deep dive into all aspects of this module is beyond the scope of the course, we will cover how to install and configure it, and demonstrate its impact on a simple webpage.

### Finding PageSpeed Information

1. **Google's Developer Site**:
    - Visit [developers.google.com/speed](https://developers.google.com/speed) to find information on PageSpeed modules.
    - This site links to [ngxpagespeed.com](https://ngxpagespeed.com/), where you'll find detailed documentation and configuration guides.
2. **Documentation and Configuration**:
    - Start with the documentation to understand how the module works and how to configure it.
    - The default page lists various filters used by PageSpeed, allowing you to select which ones to enable after installation.

### Installing PageSpeed

Unlike the ModSecurity installation, which was complex, installing PageSpeed is straightforward. Google provides a convenient bash script for installation.

1. **Prepare Installation**:
    - Open the terminal and navigate to the `/opt` directory where source code is typically stored.
2. **Run the Installation Script**:
    - Use `curl` to download and execute the installation script:
        
        ```bash
        cd /opt
        curl -f -L -sS <https://ngxpagespeed.com/install> | bash
        ```
        
    - Add `-help` to see the available options:
        
        ```
        curl -f -L -sS <https://ngxpagespeed.com/install> --help
        
        ```
        
3. **Specify Installation Options**:
    - Define a build directory, set it as a dynamic module, and use the latest stable version:
        
        ```bash
        curl -f -L -sS <https://ngxpagespeed.com/install> --build-dir=/opt --dynamic --ngx-pagespeed-version=latest-stable
        ```
        
4. **Compile the Module**:
    - Navigate to the NGINX source code directory and configure the build:
        
        ```
        cd /opt/nginx-1.12.2
        ./configure --with-compat --add-dynamic-module=/path/to/ngx_pagespeed
        make modules
        ```
        
    - The compiled module (`ngx_pagespeed.so`) will be found in the `objs` directory. Copy it to the NGINX modules directory:
        
        ```
        cp objs/ngx_pagespeed.so /etc/nginx/modules
        ```
        

### Configuring NGINX to Use PageSpeed

1. **Load the Module**:
    - Edit the `nginx.conf` file to load the PageSpeed module:
        
        ```
        load_module modules/ngx_pagespeed.so;
        
        ```
        
2. **Basic PageSpeed Configuration**:
    - Enable PageSpeed in the server block:
        
        ```bash
        pagespeed on;
        pagespeed FileCachePath /var/cache/nginx/ngx_pagespeed_cache;
        ```
        
3. **Create Cache Directory**:
    - Ensure the cache directory exists and set the correct permissions:
        
        ```bash
        mkdir -p /var/cache/nginx/ngx_pagespeed_cache
        chown -R nginx:nginx /var/cache/nginx/ngx_pagespeed_cache
        ```
        
4. **Test Configuration**:
    - Validate the NGINX configuration and reload the service:
        
        ```
        nginx -t
        systemctl reload nginx
        
        ```
        

### Testing PageSpeed

1. **Initial Performance Measurement**:
    - Disable browser caching, load the webpage, and note the initial load times.
2. **Enable PageSpeed and Measure Again**:
    - Re-enable PageSpeed, reload the webpage, and compare the load times.
3. **Observe Changes**:
    - Note the differences in load times and the size of resources. PageSpeed optimizes content by rewriting it to reduce bandwidth and improve speed.

PageSpeed is a powerful module that optimizes web content for better performance. It offers numerous filters to tweak and enhance web delivery, reducing load times and improving user experience. Although we only scratched the surface in this video, further reading on [ngxpagespeed.com](https://ngxpagespeed.com/) will provide comprehensive guidance for fine-tuning this tool to meet your specific needs.