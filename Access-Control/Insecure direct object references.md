# IDOR via Predictable File Name

## উদ্দেশ্য

যদি Sensitive File-এর নাম Sequential বা Predictable হয় এবং Access Control না থাকে, তাহলে Filename পরিবর্তন করে অন্য ব্যবহারকারীর File পড়া সম্ভব।

---

## পূর্বশর্ত

- Transcript/File Public URL-এ সংরক্ষিত।
- Filename Incremental (যেমন `1.txt`, `2.txt`, `3.txt`)।
- Server File Access-এর সময় Authorization Check করে না।

---

## আক্রমণের ধাপ

### ১. নিজের Transcript খুলুন

উদাহরণ:

```
/download-transcript/5.txt
```

---

### ২. Filename পরিবর্তন করুন

```
/download-transcript/1.txt
```

Request পাঠান।

---

### ৩. Response বিশ্লেষণ করুন

যদি Access Control না থাকে, তাহলে অন্য ব্যবহারকারীর Transcript দেখা যাবে, যেখানে Sensitive তথ্য (যেমন Password) থাকতে পারে।

---

## কেন এটি কাজ করে

Server শুধুমাত্র Filename দেখে File প্রদান করে, কিন্তু Request করা User সেই File দেখার অনুমতি রাখে কিনা তা যাচাই করে না। Predictable Filename থাকায় অন্য File সহজেই অনুমান করা যায়।

---

## Impact

- Sensitive Information Disclosure
- Credential Leakage
- Account Takeover

---

## Mitigation

- প্রতিটি File Access-এর আগে Server-side Authorization Check করতে হবে।
- Predictable Filename-এর পরিবর্তে Random UUID ব্যবহার করতে হবে।
- Sensitive File Public Path-এ সংরক্ষণ করা যাবে না।
