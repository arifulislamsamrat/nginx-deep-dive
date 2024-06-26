# Examining Load Balancing Methods Bangla

এই ভিডিওতে, আমরা NGINX এর মাধ্যমে লোড ব্যালেন্সিং কিভাবে পরিবর্তন করা যায় তা নিয়ে আলোচনা করব। বিভিন্ন ডিরেক্টিভ এবং কনফিগারেশন ব্যবহার করে, আমরা কিভাবে ট্রাফিক সার্ভারগুলির মধ্যে বিতরণ করা যায় তা নির্ধারণ করতে পারি। এই ধারণাগুলি বাস্তব উদাহরণের মাধ্যমে এবং ডকুমেন্টেশনের রেফারেন্সের মাধ্যমে ব্যাখ্যা করব, যা আমরা আমাদের বিদ্যমান `photos.example.com` আপস্ট্রিম ডিরেক্টিভে প্রয়োগ করব।

### NGINX লোড ব্যালেন্সিং মেথড

NGINX বিভিন্ন ধরনের লোড ব্যালেন্সিং পদ্ধতি প্রদান করে, যা বিভিন্ন পরিস্থিতির জন্য উপযোগী:

1. **ডিফল্ট (রাউন্ড রবিন)**
    - যদি কোন নির্দিষ্ট লোড ব্যালেন্সিং পদ্ধতি নির্ধারিত না থাকে, তবে এটি স্বয়ংক্রিয়ভাবে ব্যবহার করা হয়।
    - এটি সমস্ত সার্ভারের মধ্যে ট্রাফিক সমানভাবে বিতরণ করে।
2. **হ্যাশ**
    - একটি নির্দিষ্ট কী এর উপর ভিত্তি করে ট্রাফিক রাউট করে, যেমন `$request_uri`।
    - পুনরাবৃত্ত অনুরোধগুলি একই সার্ভারে প্রেরণ করা হয়।
    - **ঝুঁকি**: যদি একটি URI বেশিরভাগ ট্রাফিক পায়, তবে অসমান ট্রাফিক বিতরণ হতে পারে।
    - **ব্যবহার ক্ষেত্র**: অ্যাডমিন এলাকাটির ট্রাফিক নির্দিষ্ট সার্ভারে প্রেরণ করা।
3. **IP হ্যাশ**
    - ক্লায়েন্টের IP ঠিকানার (প্রথম তিন অক্টেট) উপর ভিত্তি করে ট্রাফিক রাউট করে।
    - একই নেটওয়ার্কের ক্লায়েন্টদের একই সার্ভারে রাউট করে।
    - **ঝুঁকি**: যদি একই নেটওয়ার্ক থেকে অনেক ক্লায়েন্ট থাকে তবে একটি সার্ভার ওভারলোড হতে পারে।
4. **লিস্ট কানেকশন (least_conn)**
    - সবচেয়ে কম সক্রিয় সংযোগ থাকা সার্ভারে ট্রাফিক রাউট করে।
    - রিয়েল-টাইম সার্ভার কার্যকলাপের উপর ভিত্তি করে লোড ব্যালেন্স করে।
    - বিভিন্ন অনুরোধের দৈর্ঘ্যের পরিবেশে কার্যকর।
5. **NGINX প্লাস এক্সক্লুসিভ: লিস্ট টাইম**
    - গড় প্রতিক্রিয়া সময়ের উপর ভিত্তি করে ট্রাফিক রাউট করে।
    - দ্রুত সাড়া প্রদানকারী সার্ভারে ট্রাফিক পাঠিয়ে কর্মক্ষমতা অপ্টিমাইজ করে।

### লোড ব্যালেন্সিং মেথড কনফিগার করা

লোড ব্যালেন্সিং পদ্ধতি পরিবর্তন করতে, আপনার NGINX কনফিগারেশনের আপস্ট্রিম ব্লকটি সংশোধন করুন:

- **IP হ্যাশের উদাহরণ কনফিগারেশন:**
    
    ```bash
    upstream photos {
        ip_hash;  # IP হ্যাশ পদ্ধতি ব্যবহার করা
        server server1;
        server server2;
        server server3;
    }
    ```
    
- **আর্গুমেন্ট সহ হ্যাশ মেথড:**
    
    ```bash
    upstream photos {
        hash $request_uri;  # রিকোয়েস্ট URI কী হিসেবে ব্যবহার করা
        server server1;
        server server2;
        server server3;
    }
    ```
    

**সেরা অনুশীলন**: আপস্ট্রিম ব্লকের শীর্ষে লোড ব্যালেন্সিং ডিরেক্টিভটি রাখুন যাতে স্পষ্টতা ও সঠিক কার্যকারিতা নিশ্চিত হয়।

### ট্রাফিক ম্যানেজমেন্টের জন্য সার্ভার প্যারামিটার

NGINX বিভিন্ন সার্ভার প্যারামিটার মাধ্যমে লোড ব্যালেন্সিং আরও কাস্টমাইজ করার অনুমতি দেয়:

- **ওজন (Weight)**
    - ট্রাফিক বিতরণের জন্য অগ্রাধিকার সেট করে।
    - উচ্চ ওজন মানে উচ্চ অগ্রাধিকার।
    - **উদাহরণ**: `server server1 weight=5;`
- **সর্বাধিক সংযোগ (max_conns)**
    - একটি সার্ভারে সর্বাধিক সংযোগের সীমা নির্ধারণ করে।
    - **উদাহরণ**: `server server1 max_conns=100;`
- **ব্যাকআপ**
    - একটি সার্ভারকে ব্যাকআপ হিসাবে নির্ধারণ করে, শুধুমাত্র যখন প্রধান সার্ভারগুলি ব্যর্থ হয় তখন ব্যবহৃত হয়।
    - **উদাহরণ**: `server server3 backup;`
- **ডাউন**
    - একটি সার্ভারকে অপ্রাপ্য হিসাবে চিহ্নিত করে, যা রক্ষণাবেক্ষণের জন্য দরকারী।
    - **উদাহরণ**: `server server2 down;`

### প্যাসিভ হেলথ চেকিং

প্যাসিভ হেলথ চেকিং সার্ভারের স্বাস্থ্য পর্যবেক্ষণ করে ব্যর্থতাগুলি ট্র্যাক করে:

- **ম্যাক্স ফেলস (max_fails)**
    - নির্দিষ্ট করে যে কতগুলি ব্যর্থ অনুরোধ সার্ভারটিকে অপ্রাপ্য হিসাবে চিহ্নিত করবে।
    - **উদাহরণ**: `max_fails=3;`
- **ফেল টাইমআউট (fail_timeout)**
    - ব্যর্থতাগুলি গণনা করার সময়কাল এবং সার্ভারটিকে অপ্রাপ্য হিসাবে চিহ্নিত করার সময় নির্ধারণ করে।
    - **উদাহরণ**: `fail_timeout=20s;`

এই সেটিংসগুলি NGINX কে সাময়িকভাবে একটি সার্ভারকে পুল থেকে সরিয়ে দিতে অনুমতি দেয় যদি এটি ব্যর্থ হয়, যা সার্ভারটিকে পুনরুদ্ধার করার সময় দেয়।

NGINX চারটি লোড ব্যালেন্সিং পদ্ধতি সমর্থন করে: রাউন্ড রবিন (ডিফল্ট), ip_hash, হ্যাশ, এবং least_conn। এই পদ্ধতিগুলি সার্ভার প্যারামিটার যেমন weight, max_conns, backup, এবং down ব্যবহার করে কাস্টমাইজ করা যায়। এছাড়াও, প্যাসিভ হেলথ চেকিং এর সাথে max_fails এবং fail_timeout ব্যবহার করে নির্ভরযোগ্যতা বাড়ানো যায়, যা নিশ্চিত করে যে শুধুমাত্র সুস্থ সার্ভারগুলি ব্যবহৃত হয়। এই পদ্ধতি এবং কনফিগারেশনগুলি বুঝে এবং প্রয়োগ করে, আপনি আপনার NGINX সেটআপের মধ্যে ট্রাফিক বিতরণ এবং সার্ভারের কার্যক্ষমতা অপ্টিমাইজ করতে পারেন।