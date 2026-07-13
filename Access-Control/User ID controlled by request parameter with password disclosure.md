# IDOR Leading to Credential Disclosure

## উদ্দেশ্য

যদি User Identifier পরিবর্তন করে অন্য ব্যবহারকারীর Account Data দেখা যায় এবং Response-এ Password-এর মতো Sensitive তথ্য থাকে, তাহলে সম্পূর্ণ Account Compromise সম্ভব।

---

## পূর্বশর্ত

- URL বা Parameter-এ User Identifier থাকে।
- Server Object-Level Authorization করে না।
- Response-এ Sensitive তথ্য (যেমন Password) থাকে।

---

## আক্রমণের ধাপ

### ১. নিজের Account Request ধরুন

```
GET /my-account?id=user
```

---

### ২. Identifier পরিবর্তন করুন

```
GET /my-account?id=administrator
```

Request পাঠান।

---

### ৩. Response বিশ্লেষণ করুন

যদি Authorization Check না থাকে, তাহলে Administrator-এর Account তথ্য (যেমন Password) Response-এ দেখা যাবে।

---

### ৪. Credential ব্যবহার করুন

প্রাপ্ত Credential দিয়ে Administrator Account-এ Login করা সম্ভব।

---

## কেন এটি কাজ করে

Server শুধুমাত্র `id` Parameter দেখে User-এর তথ্য ফেরত দেয়, কিন্তু Request করা User সেই তথ্য দেখার অনুমতি রাখে কিনা তা যাচাই করে না। এছাড়া Response-এ অপ্রয়োজনীয় Sensitive তথ্যও প্রকাশ করে।

---

## Impact

- Credential Disclosure
- Account Takeover
- Unauthorized Admin Access
- Full System Compromise

---

## Mitigation

- প্রতিটি Object Access-এর আগে Server-side Authorization Check করতে হবে।
- Password বা অন্যান্য Sensitive তথ্য কখনও Response-এ ফেরত দেওয়া যাবে না।
- Least Privilege এবং Object-Level Access Control প্রয়োগ করতে হবে।
