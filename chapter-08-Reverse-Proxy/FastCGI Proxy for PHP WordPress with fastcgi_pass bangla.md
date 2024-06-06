# FastCGI Proxy for PHP/WordPress with fastcgi_pass ( Bangla )

### WordPress অ্যাপ্লিকেশনের জন্য NGINX ভার্চুয়াল হোস্ট কনফিগারেশন

এই ভিডিওতে, আমরা পূর্ববর্তী ভিডিওতে সেটআপ করা WordPress অ্যাপ্লিকেশনটির জন্য একটি ভার্চুয়াল হোস্ট সার্ভার কনফিগারেশন সেটআপ করব যাতে NGINX কে এর জন্য রিভার্স প্রক্সি হিসেবে ব্যবহার করা যায়। এটি FastCGI ব্যবহার করে সোকেটের মাধ্যমে যোগাযোগ করবে এবং আমরা nginx_http_proxy_module এর পরিবর্তে nginx_http_fastcgi_module ব্যবহার করব।

### FastCGI মডিউল এবং HTTP প্রক্সি মডিউল

এখানে দুটি মডিউলের মধ্যে প্রায় কোনো পার্থক্য নেই। proxy_pass এবং fastcgi_pass উভয়ই প্রায় একইভাবে কাজ করে, তবে fastcgi_pass HTTP-এর পরিবর্তে FastCGI প্রোটোকলের মাধ্যমে যোগাযোগ করে।

### FastCGI কনফিগারেশন

1. **কনফিগারেশন ফাইল তৈরি করা**:
    
    ```
    vim /etc/nginx/conf.d/blog.example.com.conf
    ```
    
    এই ফাইলটি একটি সার্ভার ব্লক থাকবে যা 80 পোর্টে শুনবে এবং সার্ভার নাম [blog.example.com](http://blog.example.com/) সেট করবে।
    
2. **রুট ডিরেক্টরি এবং লোকেশন ব্লক সেটআপ করা**:
    
    ```
    server {
        listen 80;
        server_name blog.example.com;
    
        root /var/www/blog.example.com;
    
        location / {
            try_files $uri $uri/ /index.php?$args;
        }
    
        location ~ \\.php$ {
            fastcgi_index index.php;
            fastcgi_pass unix:/var/run/php-fpm.sock;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        }
    }
    
    ```
    
3. **fastcgi_params ফাইল সম্পাদনা করা**:
    
    ```
    vim /etc/nginx/fastcgi_params
    
    ```
    
    এই ফাইলে FastCGI প্রোটোকল অনুযায়ী প্যারামিটারগুলি নির্ধারণ করা হয়।
    

### সার্ভার পুনরায় লোড এবং /etc/hosts ফাইল আপডেট করা

1. **NGINX পুনরায় লোড করা**:
    
    ```
    systemctl reload nginx
    
    ```
    
2. **/etc/hosts ফাইল আপডেট করা**:
    
    ```
    sudo vim /etc/hosts
    
    ```
    
    এখানে `blog.example.com` যোগ করুন যাতে আপনি আপনার ব্রাউজারে এটি অ্যাক্সেস করতে পারেন।
    

### WordPress সেটআপ এবং কনফিগারেশন

1. **WordPress ইনস্টলেশন শেষ করা**:
    - আপনার ব্রাউজারে `blog.example.com` এ যান।
    - WordPress সেটআপ উইজার্ড সম্পূর্ণ করুন এবং একটি টেস্ট ব্লগ তৈরি করুন।
    - একটি ব্যবহারকারীর নাম এবং পাসওয়ার্ড সেট করুন।
    - ইনস্টলেশন শেষ করে admin ড্যাশবোর্ডে লগইন করুন।

NGINX এর প্রক্সি_pass এবং fastcgi_pass এর মধ্যে প্রধান পার্থক্য হল HTTP এর পরিবর্তে FastCGI প্রোটোকল ব্যবহার করা। এটি প্রক্সি সেটআপ করার সময় HTTP হেডারের পরিবর্তে FastCGI প্যারামিটারগুলি ব্যবহার করে। FastCGI ব্যবহার করে সোকেটের মাধ্যমে সরাসরি যোগাযোগ করা আরও কার্যকর হতে পারে কারণ এটি নেটওয়ার্ক স্ট্যাক বাইপাস করে।