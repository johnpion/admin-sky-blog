---
layout: default
title: Список файлов
---

# Список файлов

{% for file in site.static_files %}
  {% if file.extname == ".md" %}
    - [{{ file.basename }}]({{ file.path }})
  {% endif %}
{% endfor %}
