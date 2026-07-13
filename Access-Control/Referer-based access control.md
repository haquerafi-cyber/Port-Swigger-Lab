# Referer-Based Access Control Bypass

## উদ্দেশ্য

যদি Server শুধুমাত্র `Referer` Header দেখে Authorization সিদ্ধান্ত নেয়, তাহলে বৈধ `Referer` যোগ করে Access Control বাইপাস করা সম্ভব।

---

## পূর্বশর্ত

- Sensitive Endpoint `Referer` Header-এর উপর নির্ভর করে।
- Server প্রকৃত User Role যাচাই করে না।
- বৈধ Session থাকলেই Request গ্রহণ করে।

---

## আক্রমণের ধাপ

### ১. Admin Request সংগ্রহ করুন

উদাহরণ:

```
GET /admin-roles?username=carlos&action=upgrade
Referer: https://target.com/admin
```

---

### ২. সাধারণ User-এর Session ব্যবহার করুন

Admin Session Cookie-এর পরিবর্তে সাধারণ User-এর Session Cookie বসান এবং `username` নিজের Username করুন।

```
GET /admin-roles?username=user&action=upgrade
Cookie: session=user-session
Referer: https://target.com/admin
```

Request পাঠান।

---

### ৩. ফলাফল যাচাই করুন

যদি Request সফল হয়, তাহলে সাধারণ User-এর Privilege পরিবর্তিত হবে।

---

## কেন এটি কাজ করে

Server `Referer` Header-কে Trusted ধরে এবং মনে করে Request Admin Panel থেকেই এসেছে। কিন্তু `Referer` Client-Controlled Header, তাই এটি সহজেই পরিবর্তন করা যায়।

---

## Impact

- Access Control Bypass
- Privilege Escalation
- Unauthorized Administrative Action

---

## Mitigation

- Authorization-এর জন্য কখনও `Referer` Header ব্যবহার করা যাবে না।
- প্রতিটি Sensitive Request-এ Server-side Role ও Permission যাচাই করতে হবে।
- `Referer` শুধুমাত্র Logging বা Analytics-এর জন্য ব্যবহার করা উচিত, Security Control হিসেবে নয়।
