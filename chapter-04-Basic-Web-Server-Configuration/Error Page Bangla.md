# Error Page Bangla

এই ভিডিওতে আমরা NGINX-এ কিভাবে কাস্টমাইজ করা যায় যখন কোন সমস্যা হয়, বিশেষ করে সার্ভার এরর যেমন 500 এরর এবং 404 এরর-এর সময় ব্যবহারকারীর জন্য কি প্রদর্শিত হয় তা নিয়ে আলোচনা করব। যখন একজন ব্যবহারকারী 404 এরর-এর সম্মুখীন হন, এর অর্থ হলো তারা যে বিষয়বস্তু খুঁজছেন তা পাওয়া যাচ্ছে না। বর্তমানে, NGINX-এর একটি ডিফল্ট 404 পৃষ্ঠা রয়েছে, তবে আমরা এটিকে কাস্টমাইজ করতে পারি এবং অবশ্যই করা উচিত আমাদের ওয়েব সার্ভিস বা ওয়েবসাইটের জন্য ব্র্যান্ডিং বজায় রাখার জন্য।

প্রথমে, আমরা কী ঘটে যখন একটি 404 এরর ঘটে তা পরীক্ষা করব **`curl localhost`** কমান্ড ব্যবহার করে কোনও বিদ্যমান না থাকা ফাইল, যেমন **`fake.html`**, দিয়ে। NGINX ডিফল্টভাবে একটি মৌলিক 404 পৃষ্ঠা প্রদান করে, তবে আমরা এটি আমাদের কাস্টম পৃষ্ঠা দিয়ে ওভাররাইড করতে পারি। এই পৃষ্ঠাটি একটি ওয়েব ব্রাউজারে দেখা গেলে এটি অন্য কিছু পরিষেবা থেকে পরিচিত হতে পারে, তবে আমরা আমাদের সাইটের জন্য একটি কাস্টম সংস্করণ চাই।

এরপর, আমরা সার্ভার ব্লকে **`conf.d/default.conf`** ফাইলটি সম্পাদনা করে কাস্টম এরর পৃষ্ঠা সেটআপ করব। NGINX ডকুমেন্টেশন দেখে, আমরা **`error_page`** ডিরেক্টিভটি খুঁজে পাই। এই ডিরেক্টিভটি আমাদের HTTP স্ট্যাটাস কোডের একটি তালিকা এবং সংশ্লিষ্ট URI নির্দিষ্ট করতে দেয় একটি কাস্টম পৃষ্ঠা প্রদর্শনের জন্য। উদাহরণস্বরূপ, 404 পৃষ্ঠা কাস্টমাইজ করতে, আমরা আমাদের সার্ভার ব্লকে **`error_page 404 /404.html`** লাইনটি যোগ করব। এরপর, **`/usr/share/nginx/html/404.html`** ফাইলটি একটি সাধারণ HTML স্ট্রাকচারের সাথে তৈরি করতে হবে, যেমন **`<h1>404 Page Not Found</h1>`**।

এই পরিবর্তনগুলি করার পরে, আমরা **`systemctl reload nginx`** ব্যবহার করে NGINX পুনরায় লোড করব। এখন, যখন আমরা **`localhost/fake.html`** curl করি, আমরা আমাদের কাস্টম 404 পৃষ্ঠাটি দেখতে পাব।

অবশেষে, আমরা 500 থেকে 504 সার্ভার এরর-এর জন্য কাস্টম এরর পৃষ্ঠা সেটআপ করব। সার্ভার ব্লকে **`error_page 500 501 502 503 504 /50x.html`** লাইনটি যোগ করে, আমরা নিশ্চিত করব যে এই এররগুলির যে কোনও একটি ঘটলে **`/50x.html`** পৃষ্ঠাটি রেন্ডার হবে। যেহেতু **`50x.html`** পৃষ্ঠা ইতিমধ্যেই রয়েছে, আমরা আমাদের পরিবর্তনগুলি সংরক্ষণ করব এবং প্রয়োগ করতে NGINX পুনরায় লোড করব। যদিও আমরা এখানে 500 এররটি সিমুলেট করব না, আমরা বিশ্বাস করি যে সেটআপটি 404 পৃষ্ঠার মতো কাজ করবে।

এই ধাপগুলি অনুসরণ করে, আমরা কাস্টম এরর পৃষ্ঠা প্রদর্শন করে একটি ভালো ব্যবহারকারীর অভিজ্ঞতা প্রদান করতে পারি যা আমাদের সাইটের ব্র্যান্ডিং এর সাথে সামঞ্জস্যপূর্ণ।