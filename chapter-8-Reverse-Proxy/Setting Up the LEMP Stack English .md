# Setting Up the LEMP Stack English

### Using NGINX as a Reverse Proxy with FastCGI

In this video, we're going to continue looking at using NGINX as a reverse proxy, but this time we're going to focus on how it relates to FastCGI. FastCGI is a binary protocol for exchanging data, which means the data is formatted in a way that's more efficient for machines to read rather than humans. It packages information into key-value pairs and sends it across the wire, allowing two FastCGI-compatible applications to communicate with each other.

PHP, one of the most popular web programming languages, runs WordPress, which we will be utilizing in this video. Unlike Apache, NGINX cannot process PHP on its own. Therefore, we need a separate tool to handle PHP processes, and for us, that will be the PHP-FPM package.

### Setting Up PHP-FPM

1. **Install PHP-FPM**:
We'll start by installing PHP-FPM using the `centos-release-scl` repository:
    
    ```bash
    yum install -y centos-release-scl
    yum update
    yum install -y rh-php71-php rh-php71-php-fpm rh-php71-php-mysqlnd
    ```
    
2. **Configure PHP-FPM**:
Modify the configuration to use a socket for communication with NGINX. Edit the `www.conf` file:
    
    ```bash
    vim /etc/opt/rh/rh-php71/php-fpm.d/www.conf
    ```
    
    - Change the user and group to `nginx`.
    - Update the listen directive to use a socket:
        
        ```bash
        user = nginx
        group = nginx
        listen = /var/run/php-fpm.sock
        listen.owner = nginx
        listen.group = nginx
        ```
        
3. **Start and Enable PHP-FPM**:
    
    ```bash
    systemctl start rh-php71-php-fpm
    systemctl enable rh-php71-php-fpm
    ```
    

### Setting Up MariaDB

1. **Create Repository File**:
Create a new file for the MariaDB repository:
    
    ```bash
    vim /etc/yum.repos.d/mariadb.repo
    ```
    
    Add the following content:
    
    ```bash
    [MariaDB]
    name = MariaDB
    baseurl = <http://yum.mariadb.org/10.2/centos7-amd64>
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1
    ```
    
2. **Install MariaDB**:
    
    ```bash
    yum update
    yum install -y MariaDB-server MariaDB-client MariaDB-devel MariaDB-shared
    ```
    
3. **Start and Secure MariaDB**:
    
    ```bash
    systemctl start mariadb
    systemctl enable mariadb
    mysql_secure_installation
    ```
    
    - Set a root password, remove anonymous users, disable remote root login, remove test databases, and reload privileges.
4. **Create Database and User**:
    
    ```sql
    CREATE DATABASE wordpress;
    GRANT ALL PRIVILEGES ON wordpress.* TO wpuser@localhost IDENTIFIED BY 'p@ssw0rd';
    FLUSH PRIVILEGES;
    
    ```
    

### Setting Up WordPress

1. **Download and Extract WordPress**:
    
    ```bash
    mkdir /var/www/blog.example.com
    wget <http://wordpress.org/latest.tar.gz> -O - | tar -xz -C /var/www/blog.example.com
    ```
    
2. **Configure WordPress**:
    
    ```bash
    cp /var/www/blog.example.com/wp-config-sample.php /var/www/blog.example.com/wp-config.php
    vim /var/www/blog.example.com/wp-config.php
    ```
    
    - Set the database name, user, and password:
        
        ```php
        define('DB_NAME', 'wordpress');
        define('DB_USER', 'wpuser');
        define('DB_PASSWORD', 'p@ssw0rd');
        define('DB_HOST', 'localhost');
        ```
        
    - Generate and set authentication keys and salts from [WordPress secret-key service](https://api.wordpress.org/secret-key/1.1/salt).
3. **Set Permissions**:
    
    ```bash
    chown -R nginx:nginx /var/www/blog.example.com
    ```
    

In this video, we set up the web application using PHP and PHP-FPM to prepare for demonstrating how to use a reverse proxy that communicates via FastCGI in the next video.