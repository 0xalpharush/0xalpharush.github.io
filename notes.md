---
layout: default
---


<h1>Notes</h1>
Collection of useful references and ideas that are not adequately developed.

Inspired by similar notes by:
- [Langston Barrett](https://langston-barrett.github.io/notes/)
- [Philip Zucker](https://www.philipzucker.com/)

<div class="notes">
  {% for note in site.categories.notes %}
    <article class="note">

      <h3><a href="{{ site.baseurl }}{{ note.url }}">{{ note.title }}</a></h3>

    </article>
  {% endfor %}
</div>
