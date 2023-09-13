---
layout: default
# title: Главная страница
---

<!-- # Список подстраниц -->

{% for file in site.static_files %}
  {% if file.path contains '/common/' and file.extname == ".md" %}
- [{{ file.name }}]({{ file.path }})
  {% endif %}
{% endfor %}
