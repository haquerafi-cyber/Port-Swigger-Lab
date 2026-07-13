# IDOR (Insecure Direct Object Reference)

## উদ্দেশ্য

যদি কোনো Resource-এর Access শুধুমাত্র User-Controlled Identifier (যেমন `id`) এর উপর নির্ভর করে এবং Server Ownership যাচাই না করে, তাহলে অন্য ব্যবহারকারীর তথ্য দেখা সম্ভব।

---

## পূর্বশর্ত

- URL বা Parameter-এ User Identifier থাকে।
- Server Object-Level Authorization করে না।

---

## আক্রমণের ধাপ

### ১. নিজের Profile Request ধরুন

উদাহরণ:

```
GET /my-account?id=user
```

---

### ২. Identifier পরিবর্তন করুন

```
GET /my-account?id=victim
```

Request পাঠান।

---

### ৩. Response বিশ্লেষণ করুন

যদি Server Authorization Check না করে, তাহলে ভিক্টিমের তথ্য (যেমন API Key, Profile Data) ফেরত দেবে।

---

## কেন এটি কাজ করে

Server শুধুমাত্র `id` Parameter দেখে Resource নির্বাচন করে, কিন্তু Request করা User সেই Resource দেখার অনুমতি রাখে কিনা তা যাচাই করে না।

---

## Impact

- Sensitive Information Disclosure
- Unauthorized Data Access
- Account Takeover (যদি API Key বা Token ফাঁস হয়)

---

## Mitigation

- প্রতিটি Object Access-এর আগে Server-side Authorization Check করতে হবে।
- User শুধুমাত্র নিজের Resource Access করতে পারবে।
- Direct Identifier-এর পরিবর্তে Random/Indirect Identifier ব্যবহার করা যেতে পারে।
