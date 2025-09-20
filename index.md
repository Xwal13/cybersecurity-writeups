---
layout: page
title: "Cybersecurity Writeups"
permalink: /
---

# 🔐 Cybersecurity Writeups

Welcome to my collection of **bug bounty** and **security lab writeups**.  
This site documents hands-on practice from PortSwigger, TryHackMe, HackTheBox, and real bug bounty engagements.  

---

## 📑 Latest Writeups
{% for post in site.posts limit:10 %}
- [{{ post.title }}]({{ post.url | relative_url }}) <br>
  <small>🗓️ {{ post.date | date: "%b %d, %Y" }} • Categories: {{ post.categories | join: ", " }}</small>
{% endfor %}

---

## 🧭 Navigation
- [About Me](/cybersecurity-writeups/about/)
- [All Posts](/cybersecurity-writeups/posts/)

---

✍️ Maintained by [xwal13](https://github.com/xwal13)
