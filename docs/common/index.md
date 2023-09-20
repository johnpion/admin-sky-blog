# Docs

{% for file in site.static_files %}
  {% if file.extname == ".md" %}
- [{{ file.title }}](./{{ file.path }})
  {% endif %}
{% endfor %}
