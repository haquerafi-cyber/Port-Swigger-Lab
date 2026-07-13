# IDOR with Information Disclosure in Redirect Response

## উদ্দেশ্য

যদি Unauthorized Request-এর ক্ষেত্রে Server Redirect করলেও Response Body-তে Sensitive তথ্য রেখে দেয়, তাহলে Redirect থাকা সত্ত্বেও সেই তথ্য পড়া সম্ভব।

---

## পূর্বশর্ত

- Resource Identifier (যেমন `id`) User-Controlled।
- Unauthorized Request-এ Redirect হয়।
- Redirect Response Body-তে Sensitive Data থেকে যায়।

---

## আক্রমণের ধাপ

### ১. নিজের Account Request ধরুন

```
GET /my-account?id=user
```

---

### ২. Identifier পরিবর্তন করুন

```
GET /my-account?id=carlos
```

Request পাঠান।

---

### ৩. Response বিশ্লেষণ করুন

Response-এ `302 Found` বা অন্য Redirect Status থাকতে পারে, কিন্তু **Response Body** পরীক্ষা করুন।

উদাহরণ:

```
HTTP/1.1 302 Found
Location: /

API Key: XXXXXXXXXXXXX
```

যদি Body-তে Sensitive তথ্য থাকে, তাহলে সেটি সংগ্রহ করা যাবে।

---

## কেন এটি কাজ করে

Server Redirect পাঠালেও Response তৈরির সময় Sensitive Data Body-তে যুক্ত করে ফেলে। Browser সাধারণত Redirect অনুসরণ করে, তাই এই তথ্য দেখা যায় না, কিন্তু Burp Suite-এর মতো টুল দিয়ে Response Body পড়া যায়।

---

## Impact

- Sensitive Information Disclosure
- Unauthorized Data Access
- API Key বা Token Leak

---

## Mitigation

- Unauthorized Request-এর ক্ষেত্রে কোনো Sensitive Data Response Body-তে রাখা যাবে না।
- Authorization Check সম্পন্ন হওয়ার পরই Sensitive Data তৈরি বা Return করতে হবে।
- Redirect Response সর্বদা খালি বা নিরাপদ Content সহ পাঠাতে হবে।
