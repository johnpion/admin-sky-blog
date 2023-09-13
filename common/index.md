---
layout: default
title: Common
---

# Список файлов

{% for file in site.static_files %}
  {% if file.path == 'common' and file.extname == ".md" %}
[{{ file.basename }}]({{ file.path }})  
  {% endif %}
{% endfor %}
