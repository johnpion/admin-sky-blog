# Docs

{% for file in site.static_files %}
  {% if file.extname == ".md" and file.name != "index.md" %}
- [{{ file.title }}](./{{ file.path }})
  {% endif %}
{% endfor %}
