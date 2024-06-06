# Setting  Up LEMP stack Bangla

### NGINX কে রিভার্স প্রক্সি হিসেবে ব্যবহার করা এবং FastCGI

এই ভিডিওতে, আমরা NGINX কে রিভার্স প্রক্সি হিসেবে ব্যবহার করার বিষয়ে আলোচনা চালিয়ে যাব, তবে এবার আমরা FastCGI সম্পর্কিত বিষয়গুলি দেখব। FastCGI একটি বাইনারি প্রোটোকল যা ডেটা বিনিময়ের জন্য ব্যবহৃত হয়। বাইনারি প্রোটোকল বলতে বোঝায় যে ডেটাটি এমনভাবে ফরম্যাট করা হয় যা মেশিনের জন্য পড়া সহজ এবং কার্যকর। এটি তথ্যকে কী-ভ্যালু জোড়াতে প্যাকেজ করে এবং তারপরে ওয়্যার মাধ্যমে প্রেরণ করে, যা দুটি FastCGI-সামঞ্জস্যপূর্ণ অ্যাপ্লিকেশনকে একে অপরের সাথে কথা বলতে সক্ষম করে।

PHP হল সবচেয়ে জনপ্রিয় ওয়েব প্রোগ্রামিং ভাষাগুলির মধ্যে একটি এবং এটি WordPress চালায়, যা আমরা এই ভিডিওতে ব্যবহার করব। Apache এর মতো নয়, NGINX নিজেই PHP প্রসেস করতে পারে না। তাই আমাদের PHP প্রক্রিয়াগুলি পরিচালনা করার জন্য আলাদা একটি টুলের প্রয়োজন, এবং আমাদের ক্ষেত্রে তা হবে PHP-FPM প্যাকেজ।

### PHP-FPM সেটআপ করা

1. **PHP-FPM ইনস্টল করুন**:
আমরা `centos-release-scl` রিপোজিটরি ব্যবহার করে PHP-FPM ইনস্টল করব:
    
    ```
    yum install -y centos-release-scl
    yum update
    yum install -y rh-php71-php rh-php71-php-fpm rh-php71-php-mysqlnd
    
    ```
    
2. **PHP-FPM কনফিগার করুন**:
NGINX এর সাথে সাকেট ব্যবহার করে যোগাযোগ করার জন্য কনফিগারেশনটি পরিবর্তন করুন। `www.conf` ফাইলটি সম্পাদনা করুন:
    
    ```
    vim /etc/opt/rh/rh-php71/php-fpm.d/www.conf
    
    ```
    
    - ইউজার এবং গ্রুপ পরিবর্তন করে `nginx` করুন।
    - লিসেন ডিরেক্টিভটি সাকেটে পরিবর্তন করুন:
        
        ```
        user = nginx
        group = nginx
        listen = /var/run/php-fpm.sock
        listen.owner = nginx
        listen.group = nginx
        
        ```
        
3. **PHP-FPM শুরু এবং সক্রিয় করুন**:
    
    ```
    systemctl start rh-php71-php-fpm
    systemctl enable rh-php71-php-fpm
    
    ```
    

### MariaDB সেটআপ করা

1. **রিপোজিটরি ফাইল তৈরি করুন**:
MariaDB রিপোজিটরির জন্য একটি নতুন ফাইল তৈরি করুন:
    
    ```
    vim /etc/yum.repos.d/mariadb.repo
    
    ```
    
    নিম্নলিখিত বিষয়বস্তু যোগ করুন:
    
    ```
    [MariaDB]
    name = MariaDB
    baseurl = <http://yum.mariadb.org/10.2/centos7-amd64>
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1
    
    ```
    
2. **MariaDB ইনস্টল করুন**:
    
    ```
    yum update
    yum install -y MariaDB-server MariaDB-client MariaDB-devel MariaDB-shared
    
    ```
    
3. **MariaDB শুরু এবং নিরাপদ করুন**:
    
    ```
    systemctl start mariadb
    systemctl enable mariadb
    mysql_secure_installation
    
    ```
    
    - রুট পাসওয়ার্ড সেট করুন, অ্যানোনিমাস ব্যবহারকারীদের সরান, রিমোট রুট লগইন নিষ্ক্রিয় করুন, টেস্ট ডেটাবেস সরান, এবং প্রিভিলেজ রিলোড করুন।
4. **ডেটাবেস এবং ইউজার তৈরি করুন**:
    
    ```sql
    CREATE DATABASE wordpress;
    GRANT ALL PRIVILEGES ON wordpress.* TO wpuser@localhost IDENTIFIED BY 'p@ssw0rd';
    FLUSH PRIVILEGES;
    
    ```
    

### WordPress সেটআপ করা

1. **WordPress ডাউনলোড এবং এক্সট্র্যাক্ট করুন**:
    
    ```
    mkdir /var/www/blog.example.com
    wget <http://wordpress.org/latest.tar.gz> -O - | tar -xz -C /var/www/blog.example.com
    
    ```
    
2. **WordPress কনফিগার করুন**:
    
    ```
    cp /var/www/blog.example.com/wp-config-sample.php /var/www/blog.example.com/wp-config.php
    vim /var/www/blog.example.com/wp-config.php
    
    ```
    
    - ডেটাবেস নাম, ইউজার, এবং পাসওয়ার্ড সেট করুন:
        
        ```php
        define('DB_NAME', 'wordpress');
        define('DB_USER', 'wpuser');
        define('DB_PASSWORD', 'p@ssw0rd');
        define('DB_HOST', 'localhost');
        
        ```
        
    - [WordPress secret-key service](https://api.wordpress.org/secret-key/1.1/salt) থেকে অটেনটিকেশন কী এবং সল্টগুলি জেনারেট এবং সেট করুন।
3. **পারমিশন সেট করুন**:
    
    ```
    chown -R nginx:nginx /var/www/blog.example.com
    
    ```
    

এই ভিডিওতে, আমরা একটি ওয়েব অ্যাপ্লিকেশন সেটআপ করেছি PHP এবং PHP-FPM ব্যবহার করে, যাতে আমরা পরবর্তী ভিডিওতে FastCGI এর মাধ্যমে যোগাযোগ করা একটি রিভার্স প্রক্সি ব্যবহার করার প্রদর্শনী করতে পারি।