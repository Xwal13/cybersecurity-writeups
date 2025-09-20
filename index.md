---
layout: home
title: Cybersecurity Writeups
---

## 📂 Latest Writeups

{% for post in site.posts limit:5 %}
<div class="writeup-card">
  <h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
  <p>{{ post.excerpt | strip_html | truncate: 150 }}</p>
</div>
{% endfor %}

<footer>
✍️ Maintained by <a href="https://github.com/xwal13">xwal13</a>
</footer>
