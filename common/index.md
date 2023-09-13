---
layout: default
title: Common
---

# common instructions

{% for file in site.static_files %}
  {% if file.path contains '/common' and file.extname == ".md" %}
[{{ file.basename }}]({{ file.basename }})  
  {% endif %}
{% endfor %}
