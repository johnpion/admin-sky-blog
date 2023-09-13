{% for file in site.static_files %}
  {% if file.path contains '/docs/' and file.extname == ".md" %}
    - [{{ file.name }}]({{ file.path }})
  {% endif %}
{% endfor %}
