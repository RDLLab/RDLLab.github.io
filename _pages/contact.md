---
layout: page
permalink: /contact/
title: Contact
description:
nav: true
nav_order: 6
announcements:
  enabled: false # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder
---

<!-- News -->

{% if page.announcements and page.announcements.enabled %}

<h2>
    <a href="{{ '/news/' | relative_url }}" style="color: inherit">News</a>
</h2>
{% include news.liquid limit=true %}
{% endif %}

<div class="container">
  <div class="row justify-content-center">
    <!-- Map column -->
    <div class="col-md-6 mb-3">
      <div class="embed-responsive embed-responsive-16by9">
        <iframe 
          src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3257.2434520720676!2d149.1178092755103!3d-35.275072893263115!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x6b164d5b45f23ad9%3A0xe02446525496b06f!2s108%20North%20Rd%2C%20Acton%20ACT%202601%2C%20Australia!5e0!3m2!1sen!2sid!4v1752047680441!5m2!1sen!2sid"
          width="100%" 
          height="350" 
          style="border:0;" 
          allowfullscreen="" 
          loading="lazy" 
          referrerpolicy="no-referrer-when-downgrade">
        </iframe>        
      </div>
    </div>
    <!-- Text column -->
    <div class="col-md-6 mb-3 d-flex flex-column justify-content-center">
      <div>
        <p>
          Robust Decision-making and Learning (RDL) Lab<br>
          Australian National University<br>
          108 North Road, Skaidrite Building (fka. CSIT Building) Room N323<br>
          Acton, 2601<br>
          Canberra, ACT, Australia
        </p>
        <div class="d-flex align-items-center mb-2">
          <i class="ti ti-mail mr-3" style="font-size: 30px;"></i>
          <span>email: robotics.comp.cecc@anu.edu.au</span>
        </div>
        <div class="d-flex align-items-center">
          <i class="ti ti-phone mr-3" style="font-size: 30px;"></i>
          <span>phone: +61 (02) 612 51577</span>
        </div>
      </div>
    </div>
  </div>
</div>

