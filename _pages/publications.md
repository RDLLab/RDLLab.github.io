---
layout: page
permalink: /publications/
title: publications
description: Publications by categories in reversed chronological order.
nav: true
nav_order: 4
do_not_show_post_title: true
do_not_show_post_desc: true
announcements:
  enabled: false # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder
---

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<!-- News -->

{% if page.announcements and page.announcements.enabled %}

  <h2>
    <a href="{{ '/news/' | relative_url }}" style="color: inherit">News</a>
  </h2>
  {% include news.liquid limit=true %}
{% endif %}

<div class="publications">

{% bibliography %}

</div>
