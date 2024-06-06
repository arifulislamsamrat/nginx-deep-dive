# Proxying to uWSGI Python Web Application with uwsgi_pass ( Bangla )

আমরা `nginx_http_uwsgi_module` ব্যবহার করে একটি uWSGI অ্যাপ্লিকেশনের জন্য প্রক্সি সেট আপ করব। এই সেটআপটি FastCGI এবং প্রক্সি মডিউলগুলির মতোই, কিছু সাধারণ ইন্টারফেস এবং নির্দেশনাবলী সহ। আমরা `uwsgi_pass` নির্দেশনাটি এবং NGINX দিয়ে স্ট্যাটিক অ্যাসেটগুলি পরিচালনা করার উপর ফোকাস করব।

### সেটআপ

1. **NGINX কনফিগারেশন ফাইল তৈরি করা**:
    - NGINX কনফিগারেশন ডিরেক্টরিতে যান:
        
        ```bash
        cd /etc/nginx/conf.d
        ```
        
    - আপনার ডোমেইনের জন্য একটি কনফিগারেশন ফাইল তৈরি করুন:
        
        ```bash
        nano notes.example.com.conf
        ```
        
2. **কনফিগারেশন ফাইল সম্পাদনা করা**:
    - ফাইলটিতে নিম্নলিখিত কনফিগারেশন যোগ করুন:
        
        ```
        server {
            listen 80;
            server_name notes.example.com;
        
            # স্ট্যাটিক ফাইল পরিবেশন করা
            location /static/ {
                alias /var/www/notes.example.com/static/;
            }
        
            # প্রক্সি রিকোয়েস্টগুলি uWSGI তে পাঠানো
            location / {
                include uwsgi_params;
                uwsgi_pass unix:/var/run/uwsgi/notes.sock;
            }
        }
        ```
        
3. **uwsgi_params ফাইল যাচাই করা**:
    - নিশ্চিত করুন যে `uwsgi_params` ফাইলটি উপস্থিত এবং সঠিকভাবে কনফিগার করা আছে। এই ফাইলটি NGINX নির্দেশনাগুলিকে uWSGI প্যারামিটারগুলির সাথে ম্যাপ করে।
4. **NGINX রিলোড করা**:
    - NGINX কনফিগারেশন এররগুলির জন্য চেক করুন এবং সার্ভিসটি রিলোড করুন:
        
        ```bash
        nginx -t
        systemctl reload nginx
        ```
        
5. **/etc/hosts ফাইল আপডেট করা**:
    - আপনার সার্ভারের আইপি ঠিকানার সাথে ডোমেইনটি ম্যাপ করতে `/etc/hosts` ফাইলটি পরিবর্তন করুন:
        
        ```bash
        nano /etc/hosts
        ```
        
    - নিম্নলিখিত লাইনটি যোগ করুন:
        
        ```
        127.0.0.1 notes.example.com
        ```
        
6. **SELinux সমস্যা সমাধান করা**:
    - SELinux সীমাবদ্ধতার কারণে যদি আপনি 502 Bad Gateway এরর পান, একটি SELinux পলিসি তৈরি করুন:
        
        ```bash
        grep nginx /var/log/audit/audit.log | audit2allow -m nginx > nginx.te
        checkmodule -M -m -o nginx.mod nginx.te
        semodule_package -o nginx.pp -m nginx.mod
        semodule -i nginx.pp
        semodule --enable nginx
        ```
        
    - SELinux কনটেক্সটগুলি পুনরুদ্ধার করুন:
        
        ```bash
        restorecon -Rv /var/www/notes.example.com
        restorecon -Rv /var/run/uwsgi
        ```
        
7. **অ্যাপ্লিকেশন পরীক্ষা করা**:
    - একটি ওয়েব ব্রাউজার খুলুন এবং `http://notes.example.com` এ যান। নিশ্চিত করুন যে অ্যাপ্লিকেশনটি লোড হচ্ছে এবং সঠিকভাবে স্ট্যাটিক অ্যাসেটগুলি পরিবেশন করছে।
    - নিশ্চিত করুন যে ব্যবহারকারী-সৃষ্টি করা কন্টেন্ট, যেমন নোট, সঠিকভাবে কাজ করছে।

### অতিরিক্ত তথ্য

- **স্ট্যাটিক অ্যাসেটগুলি**:
    - `make static` কমান্ডটি `/var/www/notes.example.com/static` ডিরেক্টরিতে স্ট্যাটিক ফাইলগুলি রাখে। এই ডিরেক্টরি NGINX দ্বারা স্ট্যাটিক কন্টেন্ট পরিবেশনের জন্য ব্যবহৃত হয়।
- **uWSGI সকেট**:
    - uWSGI সকেট ফাইলটি `/var/run/uwsgi/notes.sock` এ অবস্থিত। এই সকেটটি NGINX কনফিগারেশন এবং uWSGI সার্ভিস ফাইল উভয়েই উল্লেখিত।
- **রিভার্স প্রক্সি সুবিধা**:
    - একটি রিভার্স প্রক্সি সার্ভার ইন্টারনেট ট্রাফিক এবং আপনার ব্যাকএন্ড অ্যাপ্লিকেশন সার্ভারগুলির মধ্যে বসে। এটি ট্রাফিক সঠিক সকেট বা পোর্টে রুট করে, স্ট্যাটিক কন্টেন্ট পরিবেশন করে, এবং ক্যাশিংয়ের জন্য কনফিগার করা যেতে পারে।