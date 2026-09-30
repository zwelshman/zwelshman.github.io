## Blog
Posts on health data engineering, LLM tooling, and research software.

{% for post in site.posts %}
## [{{ post.title }}]({{ post.url }})
<em>{{ post.date | date: "%-d %B %Y" }}</em>

{{ post.excerpt }}

---
{% endfor %}
