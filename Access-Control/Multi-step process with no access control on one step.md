# Parameter-Based Access Control Bypass

## উদ্দেশ্য

যদি কোনো Sensitive Endpoint-এ Authorization শুধুমাত্র UI বা নির্দিষ্ট Workflow-এর উপর নির্ভর করে এবং Server Request-এর Parameter যথাযথভাবে যাচাই না করে, তাহলে সাধারণ ব্যবহারকারীও সেই Endpoint ব্যবহার করে Privilege পরিবর্তন করতে পারে।

---

## পূর্বশর্ত

- Admin Action-এর Request জানা যায়।
- সাধারণ User একই Endpoint-এ Request পাঠাতে পারে।
- Server Authorization সঠিকভাবে যাচাই করে না।

---

## আক্রমণের ধাপ

### ১. Admin Request সংগ্রহ করুন

উদাহরণ:

```
POST /admin/promote

username=carlos
```

---

### ২. Session পরিবর্তন করুন

Admin Session Cookie-এর পরিবর্তে সাধারণ User-এর Session Cookie ব্যবহার করুন এবং `username` নিজের Username করুন।

```
POST /admin/promote
Cookie: session=user-session

username=user
```

Request পাঠান।

---

### ৩. ফলাফল যাচাই করুন

যদি Request সফল হয়, তাহলে সাধারণ User-এর Privilege পরিবর্তিত হবে।

---

## কেন এটি কাজ করে

Server শুধুমাত্র Request গ্রহণ করে Action সম্পন্ন করে, কিন্তু Request পাঠানো User-এর Admin Permission আছে কিনা তা যাচাই করে না। ফলে যে কোনো Authenticated User একই Endpoint ব্যবহার করতে পারে।

---

## Impact

- Privilege Escalation
- Unauthorized Admin Access
- Unauthorized Role Modification

---

## Mitigation

- প্রতিটি Sensitive Endpoint-এ Server-side Authorization Check করতে হবে।
- শুধুমাত্র Admin Role-কে Role Management-এর অনুমতি দিতে হবে।
- UI লুকিয়ে রাখাকে Security হিসেবে ব্যবহার করা যাবে না।
