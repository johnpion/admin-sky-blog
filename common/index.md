---
layout: default
title: Common
---

# Список файлов

[..]({{ .. }})
{% for file in site.static_files %}
  {% if file.path contains '/common' and file.extname == ".md" %}
[{{ file.basename }}]({{ file.basename }})  
  {% endif %}
{% endfor %}
