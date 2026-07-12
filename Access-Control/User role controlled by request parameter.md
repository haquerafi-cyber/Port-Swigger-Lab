# Client-Side Controlled Authorization (Insecure Cookie)

## উদ্দেশ্য

যদি অ্যাপ্লিকেশন Admin Privilege নির্ধারণের জন্য Client-Side Cookie-এর উপর নির্ভর করে, তাহলে Cookie পরিবর্তন করে Unauthorized Admin Access পাওয়া যেতে পারে।

---

## পূর্বশর্ত

- Admin Status Cookie-তে সংরক্ষিত।
- Cookie Integrity (Sign/Encryption) নেই।
- Server Cookie-কে সরাসরি Trust করে।

---

## আক্রমণের ধাপ

### ১. Login করুন

Login করার পর Response-এ Cookie লক্ষ্য করুন।

```
Set-Cookie: Admin=false
```

---

### ২. Cookie পরিবর্তন করুন

Response বা Browser Cookie Edit করে পরিবর্তন করুন।

```
Admin=true
```

---

### ৩. Admin Panel Access করুন

```
GET /admin
```

Server যদি Cookie-কে Trust করে, তাহলে Admin Panel Access পাওয়া যাবে।

---

## কেন এটি কাজ করে

Server নিজে Admin Permission যাচাই না করে Client-এর পাঠানো `Admin` Cookie-এর মান বিশ্বাস করে। ফলে Cookie পরিবর্তন করলেই Privilege বেড়ে যায়।

---

## Impact

- Privilege Escalation
- Unauthorized Admin Access
- Full Administrative Control

---

## Mitigation

- Authorization কখনও Client-Side Cookie-এর উপর নির্ভর করা যাবে না।
- User Role Server-Side Session বা Database থেকে যাচাই করতে হবে।
- Cookie Sign বা Encrypt করতে হবে এবং প্রতিটি Sensitive Request-এ Server-side Authorization Check করতে হবে।
