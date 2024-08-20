---
permalink: /projects/
icon: fa-solid fa-diagram-project
order: 1
---
{% assign unique_categories = "" | split: "" %}
{% for post in site.posts %}
  {% if post.categories[0] == "projects" %}
    {% assign second_category = post.categories[1] %}
    {% unless unique_categories contains second_category %}
      {% assign unique_categories = unique_categories | push: second_category %}
    {% endunless %}
  {% endif %}
{% endfor %}

<div id="post-list" class="flex-grow-1 px-xl-1">
  {% for unique_category in unique_categories%}
    <h1 class="card-title my-2 mt-md-0">{{ unique_category }}</h1>

  {% endfor %}
</div>
