---
layout: page
title: Travel
excerpt: "Logbook of my travels for the last couple of years including some travel advices"
search_omit: true
---

{% highlight ruby %}
Logbook of my travels for the last couple of years including some travel advices
{% endhighlight %}
## Soon Available

<ul class="post-list">
{% for post in site.categories.travel %} 
  <li><article><a href="{{ site.url }}{{ post.url }}">{{ post.title }} <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span>{% if post.excerpt %} <span class="excerpt">{{ post.excerpt }}</span>{% endif %}</a></article></li>
{% endfor %}
</ul>
