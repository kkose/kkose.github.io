---
title: News
permalink: /news/
---
<ul class="dated">
{% for n in site.data.news %}<li><span class="when">{{ n.date }}</span><span>{{ n.text | markdownify | remove: "<p>" | remove: "</p>" }}</span></li>
{% endfor %}</ul>
