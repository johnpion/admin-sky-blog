# Docs

{% for file in site.static_files %}
  {% if file.extname == ".md" and file.path | contains: "/docs/" %}
- [{{ file.name }}]({{ file.path }})
  {% endif %}
{% endfor %}
