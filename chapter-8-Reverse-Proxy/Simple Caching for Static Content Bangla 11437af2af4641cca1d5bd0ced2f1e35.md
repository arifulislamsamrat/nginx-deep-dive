# Simple Caching for Static Content Bangla

আমরা NGINX দিয়ে প্রক্সি সার্ভার বা রিভার্স প্রক্সি সার্ভার সেটআপ করার বিষয়ে আলোচনা করেছি, এবং কিভাবে একটি রিভার্স প্রক্সি ক্যাশিংয়ের জন্য ব্যবহার করা যেতে পারে তা দেখেছি। ক্যাশিংয়ের মাধ্যমে সার্ভারের জানা প্রতিক্রিয়া সরাসরি সরবরাহ করে অ্যাপ্লিকেশন সার্ভারের লোড কমিয়ে পারফরম্যান্স বাড়ানো যায়। বিশেষ করে ভারী লোডের সময় এটি অত্যন্ত উপকারী হয়, কারণ NGINX একাধিক অনুরোধ একসাথে আরও ভালভাবে পরিচালনা করতে পারে, যা Ruby on Rails, Django বা PHP এর মতো ভাষায় চলা অ্যাপ্লিকেশন সার্ভারগুলির তুলনায় অনেক বেশি কার্যকর।

ক্যাশিং বাস্তবায়নের জন্য `fastcgi_cache` ব্যবহার করা হয়েছে এবং উদাহরণ হিসাবে WordPress ব্যবহৃত হয়েছে। মূল কনফিগারেশনগুলির মধ্যে রয়েছে HTTP কনটেক্সটে `fastcgi_cache_path` ব্যবহার করে ক্যাশ পাথ সংজ্ঞায়িত করা এবং অনন্য অনুরোধ সনাক্তকরণের জন্য `fastcgi_cache_key` সেট করা। ক্যাশিং প্যারামিটারগুলির মধ্যে যেমন `inactive` এবং `max_size` রয়েছে, যা নিষ্ক্রিয়তার উপর ভিত্তি করে এবং সর্বাধিক ক্যাশ আকারের ভিত্তিতে ক্যাশ মুছে ফেলার নিয়ন্ত্রণ করে।

একটি কাস্টম রেসপন্স হেডার যেমন `X-Cache-Status` যোগ করার মাধ্যমে ক্যাশ স্ট্যাটাস (HIT বা MISS) পর্যবেক্ষণ করা যেতে পারে। যখন একটি অনুরোধ ক্যাশ করা হয়, একই কনটেন্টের জন্য পরবর্তী অনুরোধগুলি দ্রুততর সাড়া দেয়, উল্লেখযোগ্যভাবে প্রতিক্রিয়া সময় কমিয়ে দেয়।

WordPress বুদ্ধিমত্তার সাথে ব্যবহারকারীর লগইন স্ট্যাটাসের উপর ভিত্তি করে ক্যাশিং নিয়ন্ত্রণের জন্য রেসপন্স হেডারগুলি ব্যবহার করে। লগইন করা থাকলে, `Cache-Control: no-cache` এর মতো হেডারগুলি পাঠানো হয়, যা গতিশীল কনটেন্টের ক্যাশিং প্রতিরোধ করে। নির্দিষ্ট পরিস্থিতিতে ক্যাশ বাইপাস বা প্রতিরোধ করার জন্য কাস্টম ভেরিয়েবল এবং কন্ডিশনাল ব্যবহার করে ক্যাশ নিয়ন্ত্রণ ম্যানুয়ালি করা যেতে পারে, যেমন অ্যাডমিন পৃষ্ঠাগুলির জন্য।

সেটআপটি ওপেন-সোর্স NGINX বনাম NGINX Plus এর সীমাবদ্ধতাগুলিকেও স্পর্শ করে, যেমন কেবল বাণিজ্যিক সংস্করণে ক্যাশ পর্জ ফাংশনালিটির প্রাপ্যতা। অবশেষে, ভবিষ্যতের সেশনে মাইক্রোক্যাশিং নিয়ে আলোচনা করার মঞ্চ প্রস্তুত করা হয়েছে, যা কার্যকরভাবে প্রায়শই পরিবর্তিত কনটেন্ট পরিচালনা করার জন্য খুব ছোট ক্যাশ সময় সেট করা জড়িত।