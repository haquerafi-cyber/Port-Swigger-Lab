- URL লুকিয়ে রাখা (Security by Obscurity)
    
    কখনও কখনও **Sensitive Functionality**-কে সহজে অনুমান করা না যায় এমন একটি URL-এর আড়ালে রাখা হয়।
    
    এটিকে **Security by Obscurity** বলা হয়।
    
    তবে শুধুমাত্র URL লুকিয়ে রাখা **কার্যকর Access Control নয়**, কারণ বিভিন্ন উপায়ে ব্যবহারকারীরা সেই URL জেনে যেতে পারে।
    
    ### উদাহরণ
    
    ধরো, একটি Application-এর Admin Panel-এর URL হলো:
    
    ```
    https://insecure-website.com/administrator-panel-yb556
    ```
    
    এই URL সাধারণভাবে অনুমান করা কঠিন হতে পারে।
    
    কিন্তু Application নিজেই যদি কোনোভাবে এই URL প্রকাশ করে, তাহলে URL লুকিয়ে রাখার কোনো মূল্য থাকে না।
    
    উদাহরণস্বরূপ, User-এর Role অনুযায়ী Interface তৈরি করার জন্য JavaScript ব্যবহার করা হয়েছে:
    
    ```jsx
    varisAdmin=false;if (isAdmin) {varadminPanelTag=document.createElement('a');adminPanelTag.setAttribute('href','https://insecure-website.com/administrator-panel-yb556');adminPanelTag.innerText='Admin panel';
    }
    ```
    
    এখানে Logic অনুযায়ী শুধুমাত্র **Admin User**-এর জন্য **Admin Panel**-এর Link দেখানো হবে।
    
    কিন্তু **JavaScript File**-টি সব User-এর Browser-এ পাঠানো হয়।
    
    ফলে সাধারণ User-ও **Source Code** বা **JavaScript File** দেখে লুকানো **Admin URL** জেনে যেতে পারে।
    
    ### মূল কথা
    
    **Security by Obscurity** শুধু URL লুকিয়ে রাখে, কিন্তু Access Control প্রয়োগ করে না।
    
    যদি Server সঠিকভাবে Permission যাচাই না করে, তাহলে URL জেনে যাওয়ার পর যে কেউ সেই Sensitive Functionality-তে Access পেতে পারে।
    

# Source Code Information Disclosure

## উদ্দেশ্য

ওয়েব পেজের **HTML বা JavaScript Source Code** থেকে লুকানো Endpoint বা Admin Path বের করে সরাসরি Access করা।

---

## পূর্বশর্ত

- Source Code-এ Sensitive তথ্য (যেমন Admin URL) প্রকাশিত থাকতে হবে।
- উক্ত Path যথাযথ Access Control দ্বারা সুরক্ষিত থাকবে না।

---

## আক্রমণের ধাপ

### ১. Page Source বা Developer Tools দেখুন

Home Page-এর HTML/JavaScript পরীক্ষা করুন।

উদাহরণ:

```jsx
var adminPanel = "/administrator-panel";
```

---

### ২. প্রকাশিত Path Access করুন

```
GET /administrator-panel
```

---

### ৩. Admin Function ব্যবহার করুন

যদি Access পাওয়া যায়, তাহলে Admin Panel-এর কার্যক্রম (যেমন User Management) ব্যবহার করা সম্ভব।

---

## কেন এটি কাজ করে

Developer সুবিধার জন্য রাখা Sensitive তথ্য (Hidden URL, API Endpoint, Debug Path) Client-side Source Code-এ থেকে যায়। Browser সব Source Download করে, তাই যে কেউ তা দেখতে পারে।

---

## Impact

- Hidden Endpoint Disclosure
- Admin Panel Exposure
- Sensitive Information Leakage
- Unauthorized Access-এর ঝুঁকি

---

## Mitigation

- Client-side Source Code-এ Sensitive তথ্য রাখা যাবে না।
- Admin Path গোপন রাখার উপর নির্ভর না করে Authentication ও Authorization ব্যবহার করতে হবে।
- Debug ও Development Code Production থেকে সরিয়ে ফেলতে হবে।
