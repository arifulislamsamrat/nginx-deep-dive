# Deploying a uWSGI Application Bangla

এই সেশনে, আমরা uWSGI বাইনারি প্রোটোকল ব্যবহার করে একটি Django 2 অ্যাপ্লিকেশন সেট আপ করব। আমরা Python 3.6 এবং ডাটাবেস হিসেবে MariaDB ব্যবহার করব। সেটআপ প্রক্রিয়াটি বেশিরভাগটাই স্বয়ংক্রিয়, কিন্তু কিছু ম্যানুয়াল স্টেপ প্রয়োজন।

### প্রয়োজনীয়তা

1. **Python 3.6 ইনস্টলেশন**:
    - yum ব্যবহার করে Python 3.6 ইনস্টল করুন:
        
        ```bash
        yum install python36 python36-devel
        
        ```
        
2. **PIP ইনস্টলেশন**:
    - একটি বুটস্ট্র্যাপিং স্ক্রিপ্ট ব্যবহার করে PIP ডাউনলোড এবং ইনস্টল করুন:
        
        ```bash
        wget <https://bootstrap.pypa.io/get-pip.py>
        python3.6 get-pip.py
        
        ```
        
    - ইনস্টলেশনের পরে বুটস্ট্র্যাপিং স্ক্রিপ্টটি মুছে ফেলুন:
        
        ```bash
        rm get-pip.py
        
        ```
        
3. **pipenv এবং requests ইনস্টল করুন**:
    - প্রজেক্ট ম্যানেজমেন্ট টুলস ইনস্টল করুন:
        
        ```bash
        pip3.6 install pipenv requests
        
        ```
        

### Django অ্যাপ্লিকেশন সেট আপ করা

1. **রিপোজিটরি ক্লোন করা**:
    - স্যাম্পল অ্যাপ্লিকেশন রিপোজিটরি ক্লোন করুন:
        
        ```bash
        git clone <repository_url> /srv/www/content-uwsgi
        
        ```
        
2. **ডাটাবেস কনফিগারেশন**:
    - MariaDB তে প্রবেশ করে নতুন ডাটাবেস এবং ইউজার তৈরি করুন:
        
        ```sql
        mysql -u root -p
        CREATE DATABASE django_notes;
        GRANT ALL PRIVILEGES ON django_notes.* TO 'notes'@'localhost' IDENTIFIED BY 'p@ssw0rd';
        FLUSH PRIVILEGES;
        
        ```
        
    - ডাটাবেস ক্রেডেনশিয়ালস মনে রাখুন:
        - **ডাটাবেসের নাম**: django_notes
        - **ইউজার**: notes
        - **পাসওয়ার্ড**: p@ssw0rd
3. **Environment ভেরিয়েবল সেটআপ**:
    - ডাটাবেস মাইগ্রেশনের জন্য প্রয়োজনীয় environment ভেরিয়েবল সেট করুন:
        
        ```bash
        export NOTES_DB=django_notes
        export NOTES_DB_USER=notes
        export NOTES_DB_PASSWORD=p@ssw0rd
        
        ```
        
4. **মেক কমান্ড রান করা**:
    - অ্যাপ্লিকেশন ডিরেক্টরিতে যান:
        
        ```bash
        cd /srv/www/content-uwsgi
        
        ```
        
    - ডিপেন্ডেন্সিস ইনস্টল করুন:
        
        ```bash
        make install
        
        ```
        
    - ডাটাবেস মাইগ্রেট করুন:
        
        ```bash
        make migrate
        
        ```
        
    - স্ট্যাটিক অ্যাসেটস জেনারেট করুন:
        
        ```bash
        make static
        
        ```
        
    - সার্ভিস সেট আপ করুন:
        
        ```bash
        make service
        
        ```
        

### সার্ভিস কনফিগার করা

1. **সার্ভিস সেট আপ**:
    - `make service` কমান্ডটি নিচের তথ্যগুলো চাইবে:
        - **হোস্ট**: [notes.example.com](http://notes.example.com/)
        - **ডাটাবেসের নাম**: django_notes
        - **ইউজার**: notes
        - **পাসওয়ার্ড**: p@ssw0rd
2. **সার্ভিস চালু এবং সক্ষম করা**:
    - uWSGI সার্ভিস চালু এবং সক্ষম করুন:
        
        ```bash
        systemctl start notes.uwsgi
        systemctl enable notes.uwsgi
        
        ```