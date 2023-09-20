# Docs

{% for file in site.static_files %}
  {% if file.extname == ".md" and file.path | contains: "/docs/" %}
- [{{ file.title }}](./{{ file.path }})
  {% endif %}
{% endfor %}
