---
layout: page
title: Notes
permalink: /notes/
---

Collection of useful references and ideas that are not adequately developed.
<div class="notes">
  {% for note in site.notes %}
    <article class="note">

      <h2><a href="{{ site.baseurl }}{{ note.url }}">{{ note.title }}</a></h2>

      <div class="entry">
        {{ note.excerpt }}
      </div>

      <a href="{{ site.baseurl }}{{ note.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>