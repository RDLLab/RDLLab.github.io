---
layout: page
title: project
permalink: /project/
description: We focus on the design and development of algorithms to enable robust decision theory becomes practical computational tools for robotics and related domain. Such computational tools will enable robots to design their own strategies, such as deciding what data to use, how to gather the data, and how to adaptively improve its strategies, so as to accomplish various tasks well, despite various modelling errors and types of uncertainty, and despite limited to no information about the system and its operating environment.
nav: true
nav_order: 1
display_categories: [active,past project]
horizontal: true
announcements:
  enabled: false # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder
---
<link rel="stylesheet" href="{{ '/assets/css/custom_css/research_page.css' | relative_url | bust_css_cache }}">

<!-- News -->

{% if page.announcements and page.announcements.enabled %}

  <h2>
    <a href="{{ '/news/' | relative_url }}" style="color: inherit">News</a>
  </h2>
  {% include news.liquid limit=true %}
{% endif %}

<!-- Video demo -->
<h2>Examples of our works:</h2>
<div class="container">
  <div class="row justify-content-center">
    <div class="col-md-9 mb-3">
      <div class="embed-responsive embed-responsive-16by9">
        <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/Z2-lOzrJbmU?si=1JbLV0yBgdo8uJjp" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
      </div>
    </div>
  </div>
</div>

<!-- pages/research.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" | reverse %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" | reverse %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
