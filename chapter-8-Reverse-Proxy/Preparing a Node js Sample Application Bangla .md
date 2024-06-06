# Preparing a Node.js Sample Application Bangla

## রিভার্স প্রক্সি সেকশন ওভারভিউ

- **ফোকাস:** `proxy_pass` এবং সংশ্লিষ্ট নির্দেশনাগুলি ব্যবহার করে HTTP এর উপর ট্রাফিক প্রক্সি করা।
- **বিকল্প প্রোটোকল:** FastCGI (PHP সার্ভারগুলির জন্য), uWSGI (Python অ্যাপ্লিকেশনের জন্য)।
- **ব্যবহারিক সেটআপ:** সফটওয়্যার স্ট্যাক সেটআপের জন্য সহগামী ভিডিও।

## উদাহরণ অ্যাপ্লিকেশন ডিপ্লয়মেন্ট

### Node.js অ্যাপ্লিকেশন: s3photoapp

- **উপাদানসমূহ:** তিনটি ওয়েব অ্যাপ্লিকেশন যা ফটো আপলোড, ম্যানিপুলেশন, এবং S3 ইন্টিগ্রেশন পরিচালনা করে।
- **ধাপসমূহ:**
    1. **Node.js সেটআপ:**
        - CentOS-এ Node.js ইনস্টল করুন (সংস্করণ 8 LTS)।
        - কমান্ড: `yum install nodejs`
    2. **প্রজেক্ট সেটআপ:**
        - ওয়েব অ্যাপ্লিকেশন কোডের জন্য `/srv/www` ডিরেক্টরি তৈরি করুন।
        - রেপোজিটরি ক্লোন করুন এবং ডিরেক্টরিতে যান।
        - Node.js ডিপেন্ডেন্সি ইনস্টল করতে `make install` রান করুন।

### AWS সেটআপ

- **সার্ভিসসমূহ:** S3 এবং DynamoDB।
- **IAM ইউজার:**
    - প্রোগ্রাম্যাটিক এক্সেস সহ নতুন IAM ইউজার `s3photos` তৈরি করুন।
    - `S3FullAccess` এবং `DynamoDBFullAccess` পারমিশন অ্যাসাইন করুন।
    - কনফিগারেশনের জন্য অ্যাক্সেস কী আইডি এবং সিক্রেট অ্যাক্সেস কী সংরক্ষণ করুন।

### Systemd সার্ভিস কনফিগারেশন

- **ফাইল তৈরি:** `/etc/systemd/system/photo-storage.service`
    - **Unit সেকশন:**
        - বর্ণনা: ফটো-স্টোরেজ Node.js সার্ভিস।
        - নেটওয়ার্ক অ্যাক্সেসের প্রয়োজন।
    - **Service সেকশন:**
        - রিস্টার্ট পলিসি: `always`
        - ইউজার এবং গ্রুপ: `nobody`
        - পরিবেশ ভেরিয়েবল: `NODE_ENV=production`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`
        - এক্সিকিউট স্টার্ট কমান্ড: `/bin/node /srv/www/s3photoapp/apps/photo-storage/server.js`
    - **Install সেকশন:**
        - সার্ভিস কনফিগার এবং সক্রিয় করুন।

### একই ধরনের সার্ভিস সেটআপ

- **ডুপ্লিকেট এবং মডিফাই:** `photo-filter` এবং `web-client` এর জন্য একই ধরনের সার্ভিস ফাইল তৈরি করুন।
    - **photo-filter.service:** স্টোরেজ থেকে ফিল্টার পরিবর্তন করুন, AWS ক্রেডেনশিয়াল সরান।
    - **web-client.service:** ওয়েব-ক্লায়েন্টে রেফারেন্স পরিবর্তন করুন এবং স্টার্ট কমান্ড সামঞ্জস্য করুন।

### সার্ভিস ম্যানেজমেন্ট

- **সার্ভিসগুলো সক্রিয় এবং চালু করুন:**
    - `photo-storage`: `systemctl enable photo-storage && systemctl start photo-storage`
    - `photo-filter`: `systemctl enable photo-filter && systemctl start photo-filter`
    - `web-client`: `systemctl enable web-client && systemctl start web-client`
- **স্ট্যাটাস চেক করুন:**
    - `systemctl status` কমান্ড ব্যবহার করে সমস্ত সার্ভিসের সঠিকভাবে চলার বিষয়টি নিশ্চিত করুন।