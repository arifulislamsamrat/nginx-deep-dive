# Adding Functionality to NGINX with Dynamic Modules  ( Bangla )

### 

এই ভিডিওতে, আমরা  তৃতীয় পক্ষের মডিউল যুক্ত করব, যা হল ModSecurity ওয়েব অ্যাপ্লিকেশন ফায়ারওয়াল। আমরা ডায়নামিক মডিউল কম্পাইল করে এবং তারপরে এটি `load_module` নির্দেশ ব্যবহার করে সংযুক্ত করব, যা আমরা পূর্ববর্তী ভিডিওতে আলোচনা করেছি।

### প্রয়োজনীয় টুলস এবং লাইব্রেরি ইনস্টল করা

1. **ডেভেলপমেন্ট টুলস ইনস্টল করা:**
    - ডেভেলপমেন্ট টুলস ইনস্টল করার জন্য `yum groupinstall 'Development tools'` চালান।
2. **প্রয়োজনীয় লাইব্রেরি ইনস্টল করা:**
    - আমরা বেশ কয়েকটি নির্ভরশীল লাইব্রেরি ইনস্টল করব। নিম্নলিখিত কমান্ডটি ব্যবহার করুন:
        
        ```bash
        yum install -y geoip-devel libcurl-devel libxml2-devel libxslt-devel libgd-devel lmdb-devel openssl-devel pcre-devel perl-ExtUtils-Embed yajl-devel zlib-devel
        
        ```
        

### ModSecurity ডাউনলোড এবং বিল্ড করা

1. **ModSecurity রেপোজিটরি ক্লোন করা:**
    - `/opt` ডিরেক্টরিতে যান এবং নিম্নলিখিত কমান্ডগুলি চালান:
        
        ```bash
        git clone --depth 1 --branch v3/master <https://github.com/SpiderLabs/ModSecurity>
        cd ModSecurity
        git submodule init
        git submodule update
        ./build.sh
        
        ```
        
2. **ModSecurity কনফিগার এবং কম্পাইল করা:**
    - ModSecurity কনফিগার এবং কম্পাইল করতে নিম্নলিখিত কমান্ডগুলি চালান:
        
        ```bash
        ./configure
        make
        make install
        
        ```
        

### ModSecurity NGINX মডিউল বিল্ড করা

1. **ModSecurity-NGINX র‌্যাপার ক্লোন করা:**
    - `/opt` ডিরেক্টরিতে ফিরে যান এবং চালান:
        
        ```bash
        git clone --depth 1 <https://github.com/SpiderLabs/ModSecurity-nginx.git>
        
        ```
        
2. **NGINX সোর্স কোড ডাউনলোড করা:**
    - আপনার NGINX সংস্করণ নির্ধারণ করতে `nginx -v` চালান এবং সংশ্লিষ্ট সোর্স কোড ডাউনলোড করুন:
        
        ```bash
        wget <http://nginx.org/download/nginx-><version>.tar.gz
        tar zxvf nginx-<version>.tar.gz
        cd nginx-<version>
        
        ```
        
3. **ডায়নামিক মডিউল কম্পাইল করা:**
    - ডায়নামিক মডিউল কম্পাইল করতে নিম্নলিখিত কমান্ডগুলি চালান:
        
        ```bash
        ./configure --with-compat --add-dynamic-module=../ModSecurity-nginx
        make modules
        cp objs/ngx_http_modsecurity_module.so /etc/nginx/modules
        
        ```
        

### NGINX কনফিগারেশনে ModSecurity মডিউল লোড করা

1. **NGINX কনফিগারেশন আপডেট করা:**
    - নতুন মডিউলটি অন্তর্ভুক্ত করতে `nginx.conf` ফাইলটি সম্পাদনা করুন:
        
        ```bash
        load_module /etc/nginx/modules/ngx_http_modsecurity_module.so;
        
        ```
        
2. **ModSecurity কনফিগারেশন সেটআপ করা:**
    - ModSecurity কনফিগারেশনের জন্য একটি ডিরেক্টরি তৈরি করুন:
        
        ```bash
        mkdir /etc/nginx/modsecurity
        cp /opt/ModSecurity/modsecurity.conf-recommended /etc/nginx/modsecurity/modsecurity.conf
        cp /opt/ModSecurity/unicode.mapping /etc/nginx/modsecurity/unicode.mapping
        
        ```
        
3. **ModSecurity কনফিগারেশন সমন্বয় করা:**
    - `/etc/nginx/modsecurity/modsecurity.conf` ফাইলটি সম্পাদনা করে অডিট লগ পাথ আপডেট করুন:
        
        ```bash
        SecAuditLog /var/log/nginx/modsec_audit.log
        
        ```
        
4. **NGINX এ ModSecurity সক্ষম করা:**
    - আপনার সার্ভার ব্লকে নিম্নলিখিত নির্দেশগুলি যোগ করুন:
        
        ```bash
        modsecurity on;
        modsecurity_rules_file /etc/nginx/modsecurity/modsecurity.conf;
        
        ```
        
5. **NGINX কনফিগারেশন যাচাই এবং রিলোড করা:**
    - কনফিগারেশন ত্রুটির জন্য NGINX যাচাই করুন:
        
        ```bash
        nginx -t
        systemctl reload nginx
        
        ```
        

আমরা সফলভাবে ModSecurity ওয়েব অ্যাপ্লিকেশন ফায়ারওয়ালকে NGINX-এ ডায়নামিক মডিউল হিসাবে যোগ করেছি। আমরা প্রয়োজনীয় সোর্স কোড ডাউনলোড এবং বিল্ড করা, মডিউল কনফিগার করা এবং এটি বিদ্যমান NGINX সেটআপে কোন ডাউনটাইম ছাড়াই সংযুক্ত করার পদ্ধতি আলোচনা করেছি। এই পদ্ধতিটি আমাদের NGINX এর কার্যকারিতা দক্ষতার সাথে ডায়নামিক মডিউল ব্যবহারে  সহায়তা করে।