# HTTP Method Override / Method-Based Access Control Bypass

## উদ্দেশ্য

যদি কোনো Endpoint শুধুমাত্র নির্দিষ্ট HTTP Method (যেমন `POST`) যাচাই করে Authorization প্রয়োগ করে, কিন্তু অন্য Method (যেমন `GET`) একই কাজ করতে দেয়, তাহলে Method পরিবর্তন করে Access Control বাইপাস করা যায়।

---

## পূর্বশর্ত

- Endpoint একাধিক HTTP Method সমর্থন করে।
- Authorization সব Method-এর জন্য সমানভাবে প্রয়োগ করা হয় না।
- একই Action `GET` এবং `POST` উভয় দিয়েই সম্ভব।

---

## আক্রমণের ধাপ

### ১. Admin Action Request সংগ্রহ করুন

উদাহরণ:

```
POST /admin/promote HTTP/1.1

username=carlos
```

---

### ২. Method পরিবর্তন করুন

Request-টি `GET`-এ রূপান্তর করুন।

```
GET /admin/promote?username=user HTTP/1.1
```

Request পাঠান।

---

### ৩. ফলাফল যাচাই করুন

যদি `GET` Request-এও একই Action সম্পন্ন হয়, তাহলে Method-Based Access Control বাইপাস হয়েছে।

---

## কেন এটি কাজ করে

Server শুধুমাত্র `POST` Request-এর ক্ষেত্রে Authorization যাচাই করে, কিন্তু `GET` Request একই Endpoint-এ পৌঁছেও সেই যাচাই এড়িয়ে যায়।

---

## Impact

- Access Control Bypass
- Unauthorized Privilege Escalation
- Unauthorized Administrative Action

---

## Mitigation

- সব HTTP Method-এর জন্য একই Authorization Check প্রয়োগ করতে হবে।
- শুধুমাত্র প্রয়োজনীয় HTTP Method Allow করতে হবে।
- State-Changing Action কখনও `GET` Method দিয়ে করা যাবে না।
- Unsupported Method-এর জন্য `405 Method Not Allowed` ফিরিয়ে দিতে হবে।
