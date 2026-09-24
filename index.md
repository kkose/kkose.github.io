---
redirect_from: /about/
---
<section class="hero">
  <img src="{{ '/images/kivanc-kose.jpg' | relative_url }}" alt="Kivanc Kose" width="600" height="600">
  <div markdown="1">
# Kivanc Kose, Ph.D.
<p class="role">Assistant Lab Member (Assistant Professor), Dermatology Service, Memorial Sloan Kettering Cancer Center<br>
Adjunct Assistant Professor, Columbia University</p>

I'm an electrical engineer who builds machine learning and computer vision tools for noninvasive cancer imaging: reflectance confocal microscopy, dermoscopy, smartphone photography and 3D total-body photography. I work across the Optical Imaging Lab and the Dermatology Imaging Informatics Lab at MSK, and I'm co-PI of the NIH-funded [International Skin Imaging Collaboration (ISIC)](https://www.isic-archive.com) Archive, the leading public skin imaging repository for AI research.

[Email](mailto:{{ site.email }}) · [Google Scholar](https://scholar.google.com/citations?user=BAQNDLAAAAAJ) · [ORCID](https://orcid.org/0000-0003-3185-2639) · [CV (PDF)]({{ '/files/Kivanc-Kose-CV.pdf' | relative_url }})
  </div>
</section>

## About

My goal is to make new imaging technologies easier to adopt in the clinic, so patients can get rapid, minimally invasive diagnosis and treatment. I care about tools that survive clinical deployment: two of my segmentation algorithms for reflectance confocal microscopy are integrated into a research RCM device at MSK for prospective evaluation.

I believe every patient should have access to high-quality care, and that AI should be part of continuing medical education so physicians can use it effectively and responsibly. I mentor medical fellows, students and colleagues at MSK in machine learning and computer vision, and I lecture and advise engineering and arts graduate students at Columbia and Northeastern.

Through ISIC I help build and analyze large, open dermatology datasets and coordinate international machine-learning challenges, so the wider research community can work on medical image analysis. I have published more than 100 collaborative papers with clinicians and engineers.

I received my B.Sc., M.Sc. and Ph.D. in Electrical and Electronics Engineering from Bilkent University, Turkey, and was a postdoctoral fellow at MSK from 2012 to 2016.

## News

<ul class="dated">
{% for n in site.data.news limit: 5 %}<li><span class="when">{{ n.date }}</span><span>{{ n.text | markdownify | remove: "<p>" | remove: "</p>" }}</span></li>
{% endfor %}</ul>
[All news →]({{ '/news/' | relative_url }})

## Selected publications

<ol class="pubs">
{% for p in site.data.publications %}{% if p.selected %}{% include pub.html p=p %}{% endif %}{% endfor %}
</ol>
[Full publication list →]({{ '/publications/' | relative_url }})
