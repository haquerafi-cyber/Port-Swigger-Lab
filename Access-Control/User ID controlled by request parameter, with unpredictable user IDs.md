# IDOR (Indirect Object Reference)

## উদ্দেশ্য

যদি Public Resource (যেমন Blog Profile) থেকে কোনো User ID জানা যায় এবং Server সেই ID-এর উপর ভিত্তি করে Sensitive Data দেখায়, তাহলে অন্য ব্যবহারকারীর তথ্য অ্যাক্সেস করা সম্ভব।

---

## পূর্বশর্ত

* User ID Publicly জানা যায়।
* Server Object-Level Authorization করে না।

---

## আক্রমণের ধাপ

### ১. Target User-এর ID সংগ্রহ করুন

Public Profile বা Blog থেকে User ID বের করুন।

উদাহরণ:

```text
GET /blogs?userId=12
```

এখানে `12` হলো Target User-এর ID।

---

### ২. নিজের Account Request ধরুন

```http
GET /my-account?id=5
```

---

### ৩. ID পরিবর্তন করুন

```http
GET /my-account?id=12
```

Request পাঠান।

---

### ৪. Response বিশ্লেষণ করুন

যদি Authorization Check না থাকে, তাহলে Target User-এর Sensitive তথ্য (যেমন API Key) পাওয়া যাবে।

---

## কেন এটি কাজ করে

Server শুধুমাত্র `id` দেখে Resource ফেরত দেয়, কিন্তু Request করা User সেই Resource দেখার অনুমতি রাখে কিনা তা যাচাই করে না।

---

## Impact

* Sensitive Information Disclosure
* Unauthorized Data Access
* Account Takeover (API Key বা Token ফাঁস হলে)

---

## Mitigation

* প্রতিটি Object Access-এর আগে Server-side Authorization Check করতে হবে।
* Public Identifier এবং Sensitive Resource-এর মধ্যে সরাসরি সম্পর্ক রাখা যাবে না।
* Predictable ID-এর পরিবর্তে Random Identifier (UUID) ব্যবহার করা উচিত।
