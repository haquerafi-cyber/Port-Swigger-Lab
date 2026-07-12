# robots.txt Information Disclosure

## উদ্দেশ্য

`robots.txt` ফাইল থেকে লুকানো বা Sensitive Directory-এর Path বের করে সরাসরি Access করা।

---

## পূর্বশর্ত

- `robots.txt` Publicly Accessible হতে হবে।
- সেখানে Sensitive Path উল্লেখ থাকতে হবে।
- সেই Path Access Control দ্বারা সুরক্ষিত থাকবে না।

---

## আক্রমণের ধাপ

### ১. `robots.txt` দেখুন

```
GET /robots.txt
```

উদাহরণ:

```
User-agent: *
Disallow: /administrator-panel
```

---

### ২. প্রকাশিত Path Access করুন

```
GET /administrator-panel
```

---

### ৩. Admin Function ব্যবহার করুন

যদি Access পাওয়া যায়, তাহলে Admin Panel-এর কার্যক্রম (যেমন User Management) ব্যবহার করা সম্ভব।

---

## কেন এটি কাজ করে

`robots.txt` শুধু Search Engine-কে কোন Path Crawl না করতে বলে। এটি কোনো Access Control নয়। তাই যে কেউ ফাইলটি পড়ে উল্লেখিত Path-এ সরাসরি যেতে পারে।

---

## Impact

- Hidden Directory Disclosure
- Admin Panel Exposure
- Sensitive Function Access
- তথ্য ফাঁস বা Account Management-এর ঝুঁকি

---

## Mitigation

- `robots.txt`এ Sensitive Path প্রকাশ করা যাবে না।
- Admin Panel-এর জন্য Authentication ও Authorization বাধ্যতামূলক করতে হবে।
- Sensitive Resource শুধুমাত্র Access Control দিয়ে সুরক্ষিত রাখতে হবে।
