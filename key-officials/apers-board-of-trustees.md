---
layout: default
title: APERS Board of Trustees
---

# Arkansas Public Employees' Retirement System (APERS) Board of Trustees

<p>This is the official board of trustees for the Arkansas Public Employees' Retirement System (APERS). This body is responsible for the fiduciary oversight of the pension fund, including the authorization of a $50 million investment in Israel Bonds on June 11, 2025.</p>

<div class="trustee-grid">
  {% for trustee in site.data.apers_trustees %}
    <div class="trustee-card modal-trigger"
         data-name="{{ trustee.name }}" 
         data-title="{{ trustee.title }}" 
         data-type="{{ trustee.type }}"
         data-email="{{ trustee.email }}"
         data-phone="{{ trustee.phone }}">
      <h3 class="trustee-name">{{ trustee.name }}</h3>
      <p class="trustee-title">{{ trustee.title }}</p>
      <p class="trustee-type">{{ trustee.type }}</p>
      <div class="trustee-contact-actions">
        <span>View Contact Info</span>
      </div>
    </div>
  {% endfor %}
</div>
