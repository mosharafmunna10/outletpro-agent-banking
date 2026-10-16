# OutletPro Online — বিনামূল্যে পরীক্ষামূলক ডেপ্লয়মেন্ট (বাংলা)

## গুরুত্বপূর্ণ সতর্কতা
এই পদ্ধতি শুধু ডেমো/পরীক্ষার জন্য। Free hosting সার্ভিস idle হলে ঘুমিয়ে যেতে পারে; Supabase Free project নিষ্ক্রিয় থাকলে pause হতে পারে এবং Free plan-এ স্বয়ংক্রিয় backup নেই। ব্যবসায়িক/গুরুত্বপূর্ণ তথ্য বা গ্রাহকের ব্যক্তিগত তথ্য রাখবেন না। এটি BRAC Bank-এর অনুমোদিত/সংযুক্ত ব্যাংকিং সিস্টেম নয়। Production ব্যবহারের আগে নিরাপত্তা পরীক্ষা, backup ও restore পরীক্ষা, এবং পূর্ণ performance dashboard ও user-management UI সম্পন্ন করতে হবে।

## যা লাগবে
- একটি GitHub account
- একটি Supabase account
- একটি Render account
সবগুলোতেই free tier দিয়ে শুরু করা যায়। অ্যাকাউন্ট খোলার সময় নিজের ইমেইল ব্যবহার করবেন। পাসওয়ার্ড/secret কাউকে দেবেন না।

## ধাপ ১ — Supabase-এ database তৈরি
1. https://supabase.com এ account খুলে New project নির্বাচন করুন।
2. Project name দিন `outletpro-online` এবং শক্তিশালী database password তৈরি করে নিরাপদে রাখুন।
3. Project তৈরি হলে SQL Editor খুলুন।
4. এই repository-র `db/schema.sql` ফাইলের সম্পূর্ণ SQL কপি করে SQL Editor-এ Run করুন। এতে ৮টি আউটলেট seed হবে।
5. Project Settings → Database/Connect থেকে PostgreSQL connection string নিন। সম্ভব হলে Session Pooler/Transaction Pooler নয়, অ্যাপের জন্য উপযুক্ত pooler connection বেছে নিন; Supabase-এর বর্তমান নির্দেশনা অনুসরণ করুন। connection string গোপন রাখুন।

## ধাপ ২ — GitHub-এ private repository
1. https://github.com এ sign up/sign in করুন।
2. New repository → নাম `outletpro-online` → **Private** নির্বাচন করে তৈরি করুন।
3. এই ZIP-এর ভিতরের `outletpro-online` ফোল্ডারের সব ফাইল ও ফোল্ডার repository-তে upload করুন। ZIP ফাইল নিজে repository-তে আপলোড করবেন না; আগে extract করুন। `.env` বা password/secret কখনো আপলোড করবেন না।

## ধাপ ৩ — Render-এ web service তৈরি
1. https://render.com এ account খুলুন এবং GitHub connect করুন।
2. New → Blueprint নির্বাচন করে আপনার private `outletpro-online` repository connect করুন। `render.yaml` থেকে free web service কনফিগার হবে।
3. `DATABASE_URL` চাইলে Supabase-এর connection string দিন। `SESSION_SECRET` Render নিজে generate করবে। কোনো secret chat-এ পাঠাবেন না।
4. Deploy শেষ হলে Render একটি `https://....onrender.com` URL দেবে। প্রথমবার খুলতে কিছু সময় লাগতে পারে কারণ free service idle হলে sleep করে।
5. `/api/health` খুলে health endpoint যাচাই করুন।

## ধাপ ৪ — প্রথম admin ও outlet user তৈরি
এখনকার Render free service-এ shell/SSH সুবিধা নাও থাকতে পারে। তাই admin/user তৈরির জন্য এই starter-এ নিরাপদ self-service admin setup UI নেই। User তৈরি করার আগে নির্ভরযোগ্য migration/one-time provisioning পদ্ধতি যোগ করতে হবে; Supabase SQL Editor-এ plain password বসাবেন না। এই কাজ না হওয়া পর্যন্ত অ্যাপটি সাধারণ ব্যবহারে চালু করবেন না।

## ফ্রি প্ল্যানের সীমাবদ্ধতা
- Render Free web service idle হলে sleep করে; পরের request ধীর হতে পারে।
- Supabase Free-তে সীমিত storage/usage, inactivity pause এবং automatic backups না থাকার সীমাবদ্ধতা আছে।
- Free plan-এর শর্ত ও সীমা বদলাতে পারে; account dashboard-এ বর্তমান শর্ত যাচাই করুন।
- এই starter-এ পূর্ণ Outlet Target vs Achievement রিপোর্ট UI এবং user-management UI এখনও সম্পূর্ণ নয়; API ও schema-এর কিছু অংশ আছে মাত্র।
