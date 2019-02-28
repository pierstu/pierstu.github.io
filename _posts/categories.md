---
layout: page
permalink: /categories/
---
##catégories


{% for category in site.categories %}
{% capture category_name %}{{ category | first}}{% endcapture %}
###{{category_name %}}
<ul>
{% for post in site.categories[category_name] %}
<li>
<span>{{ post.date | date: "%Y-%m-%-d" }}</span>
<a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
</li>
{% endfor %}
</ul>
{% endfor %}
