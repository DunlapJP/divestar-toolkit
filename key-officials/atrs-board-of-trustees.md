---
layout: default
title: ATRS Board of Trustees
---

# Arkansas Teacher Retirement System (ATRS) Board of Trustees

<p>This is the official board of trustees for the Arkansas Teacher Retirement System (ATRS). This body is responsible for the fiduciary oversight of the pension fund, including the authorization of a $50 million investment in Israel Bonds in June 2025.</p>

<div class="trustee-grid">
  {% for trustee in site.data.atrs_trustees %}
    <div class="trustee-card">
      <h3 class="trustee-name">{{ trustee.name }}</h3>
      <p class="trustee-title">{{ trustee.title }}</p>
      <p class="trustee-type">{{ trustee.type }}</p>
      <div class="trustee-contact">
        {% if trustee.email %}
          <div class="contact-info">
            <a href="mailto:{{ trustee.email }}">Email</a>
            <span>{{ trustee.email }}</span>
          </div>
        {% endif %}
        {% if trustee.phone %}
          <div class="contact-info">
            <a href="tel:{{ trustee.phone }}">Call</a>
            <span>{{ trustee.phone }}</span>
          </div>
        {% endif %}
      </div>
    </div>
  {% endfor %}
</div>
