---
title: Publications
permalink: /publications/
redirect_from:
  - /articles/
  - /articles/JID2014/
---
<p class="note">The most up-to-date list is on <a href="https://scholar.google.com/citations?user=BAQNDLAAAAAJ">Google Scholar</a> and <a href="https://orcid.org/0000-0003-3185-2639">ORCID</a>. Author lists shortened with "…" follow Google Scholar.</p>

## Selected publications

<ol class="pubs">
{% for p in site.data.publications %}{% if p.selected %}{% include pub.html p=p %}{% endif %}{% endfor %}
</ol>

## All publications

{% assign groups = site.data.publications | group_by: "year" %}
{% for g in groups %}
### {% if g.name == "" %}Undated{% else %}{{ g.name }}{% endif %}

<ul class="pubs">
{% for p in g.items %}{% include pub.html p=p %}{% endfor %}
</ul>
{% endfor %}
