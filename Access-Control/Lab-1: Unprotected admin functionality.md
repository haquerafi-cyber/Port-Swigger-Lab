### Vulnerability: This lab has an unprotected admin panel.

### Goal: Solve the lab by deleting the user `carlos`.

In this challenge, we can check what is allowed and disallowed by accessing the robots.txt file through the URL. The admin panel is marked as disallowed there, but if we manually enter the 
`/administrator-panel` URL, we can still access it. This is the vulnerability of the site. After that, we can log in as an admin and delete a user to solve the lab.

https://…./robots.txt

```
User-agent: *
Disallow: /administrator-panel
```

From this:

User-agent: *
Disallow: /administrator-panel

We can understand that the /administrator-panel path is disallowed. However, if we enter /administrator-panel in the URL, we can still log in as an admin and gain admin access.

### Conclusion: **By changing or guessing the URL, we can still gain access to the web application.**