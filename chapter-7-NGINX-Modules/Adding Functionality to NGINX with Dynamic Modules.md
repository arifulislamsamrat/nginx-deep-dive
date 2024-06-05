# Adding Functionality to NGINX with Dynamic Modules  ( English )

In this video, we are going to add our first third-party module by integrating the ModSecurity Web Application Firewall. We will do this by compiling the dynamic module and then including it using the `load_module` directive, as discussed in the previous video.

To get started, we need to install various packages using `yum` to facilitate the build process. First, we need to download the NGINX source code compatible with our running version of NGINX, then download and compile the ModSecurity source code. After that, we will use the NGINX source code to build the dynamic library using a package created by SpiderLabs, allowing us to integrate ModSecurity into NGINX using dynamic modules.

### Installing Necessary Tools and Dependencies

1. **Install Development Tools:**
    - Run `yum groupinstall 'Development tools'` to install the necessary developer tools.
2. **Install Required Libraries:**
    - We need to install several dependent libraries. Use the following command:
        
        ```bash
        yum install -y geoip-devel libcurl-devel libxml2-devel libxslt-devel libgd-devel lmdb-devel openssl-devel pcre-devel perl-ExtUtils-Embed yajl-devel zlib-devel
        
        ```
        

### Downloading and Building ModSecurity

1. **Clone ModSecurity Repository:**
    - Navigate to the `/opt` directory and run the following commands:
        
        ```bash
        git clone --depth 1 --branch v3/master <https://github.com/SpiderLabs/ModSecurity>
        cd ModSecurity
        git submodule init
        git submodule update
        ./build.sh
        
        ```
        
2. **Configure and Compile ModSecurity:**
    - Run the following commands to configure and compile ModSecurity:
        
        ```bash
        ./configure
        make
        make install
        ```
        

### Building ModSecurity NGINX Module

1. **Clone ModSecurity-NGINX Wrapper:**
    - Back in the `/opt` directory, run:
        
        ```bash
        git clone --depth 1 <https://github.com/SpiderLabs/ModSecurity-nginx.git>
        ```
        
2. **Download NGINX Source Code:**
    - Determine your NGINX version using `nginx -v` and download the corresponding source code:
        
        ```bash
        wget <http://nginx.org/download/nginx-><version>.tar.gz
        tar zxvf nginx-<version>.tar.gz
        cd nginx-<version>
        ```
        
3. **Compile the Dynamic Module:**
    - Use the following commands to compile the dynamic module:
        
        ```bash
        ./configure --with-compat --add-dynamic-module=../ModSecurity-nginx
        make modules
        cp objs/ngx_http_modsecurity_module.so /etc/nginx/modules
        ```
        

### Configuring NGINX to Load the ModSecurity Module

1. **Update NGINX Configuration:**
    - Edit the `nginx.conf` file to include the new module:
        
        ```bash
        load_module /etc/nginx/modules/ngx_http_modsecurity_module.so;
        ```
        
2. **Setup ModSecurity Configuration:**
    - Create a directory for ModSecurity configuration:
        
        ```bash
        mkdir /etc/nginx/modsecurity
        cp /opt/ModSecurity/modsecurity.conf-recommended /etc/nginx/modsecurity/modsecurity.conf
        cp /opt/ModSecurity/unicode.mapping /etc/nginx/modsecurity/unicode.mapping
        ```
        
3. **Adjust ModSecurity Configuration:**
    - Edit `/etc/nginx/modsecurity/modsecurity.conf` to update the audit log path:
        
        ```bash
        SecAuditLog /var/log/nginx/modsec_audit.log
        ```
        
4. **Enable ModSecurity in NGINX:**
    - In your server block, add the following directives:
        
        ```bash
        modsecurity on;
        modsecurity_rules_file /etc/nginx/modsecurity/modsecurity.conf;
        ```
        
5. **Verify and Reload NGINX:**
    - Check the NGINX configuration for errors:
        
        ```bash
        nginx -t
        systemctl reload nginx
        ```
        

### Conclusion

In this video, we have successfully added the ModSecurity Web Application Firewall as a dynamic module in NGINX. We covered downloading and building the necessary source code, configuring the module, and integrating it into our existing NGINX setup without any downtime. This method allows us to enhance the functionality of NGINX efficiently by utilizing dynamic modules.