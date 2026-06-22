---
layout: home
author_profile: true
---

{% include figure popup=true image_path="/assets/images/int_tri.png" alt="Coarsened intrinsic triangulation of Spot." caption="The coarsened intrinsic triangulation of *Spot* in yellow compared to the original triangulation in black." %}

Welcome! I am a Mathematics graduate from Reed College with a minor in Computer Science residing in the San Francisco Bay Area. At Reed, I did extensive research on **geometry processing** and applications in both **deep learning** and **general-purpose GPU programming**.

---

## Recent Projects

{% assign projects = site.projects | sort: 'date' | reverse %}
<div class="entries-grid">
  {% include documents-collection.html entries=projects type="grid" %}
</div>

---

## Recent Blog Posts

{% assign posts = site.posts | sort: 'date' | reverse %}
<div class="entries-grid">
  {% include documents-collection.html entries=posts type="grid" %}
</div>