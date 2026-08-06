---
permalink: /
title: "Yan Luo"
author_profile: false
redirect_from:
  - /about/
  - /about.html
---

{% include base_path %}
{% assign group_photo = '/images/profile.jpg' | relative_url %}
{% assign recent_news = site.data.news | sort: 'date' | reverse %}
{% assign research_highlights_media_url = '/research-highlights/media/' | relative_url %}
{% assign sorted_publications = site.publications | sort: 'date' | reverse %}
{% assign author_aliases = site.author.name | downcase | append: "|luo y|y luo|luo yan|罗颜" | split: "|" %}
{% assign manual_news_urls = "" | split: "" %}
{% for item in recent_news limit:5 %}
  {% if item.url %}
    {% assign item_url_array = item.url | downcase | split: "||" %}
    {% assign manual_news_urls = manual_news_urls | concat: item_url_array %}
  {% endif %}
{% endfor %}
{% assign recent_news_visible_count = recent_news | size %}
{% if recent_news_visible_count > 5 %}
  {% assign recent_news_visible_count = 5 %}
{% endif %}
{% assign publication_news_count = 0 %}
{% for post in sorted_publications %}
  {% assign post_date = post.date | date: "%Y-%m-%d" %}
  {% assign post_paperurl = post.paperurl | default: "" | downcase %}
  {% unless manual_news_urls contains post_paperurl %}
  {% unless post_date == '2021-12-17' and post.venue == 'Innovation in Aging' %}
  {% if post.category == 'publication' and post.date and post_date != '1900-01-01' and post.authors %}
    {% assign publication_authors = post.authors | strip_html | split: ',' %}
    {% assign show_publication = false %}
    {% for raw_author in publication_authors limit: 2 %}
      {% assign candidate_author = raw_author | strip | downcase %}
      {% for alias in author_aliases %}
        {% if candidate_author == alias %}
          {% assign show_publication = true %}
        {% endif %}
      {% endfor %}
    {% endfor %}

    {% if show_publication %}
      {% assign publication_news_count = publication_news_count | plus: 1 %}
    {% endif %}
  {% endif %}
  {% endunless %}
  {% endunless %}
{% endfor %}
{% assign recent_news_total_count = recent_news_visible_count | plus: publication_news_count %}
{% assign highlight_multiomics = nil %}
{% assign highlight_internet = nil %}
{% for post in sorted_publications %}
  {% if post.title == 'AI-based multiomics profiling reveals complementary omics contributions to personalized prediction of cardiovascular disease' %}
    {% assign highlight_multiomics = post %}
  {% endif %}
  {% if post.title contains 'Positive association between Internet use and mental health among adults aged' %}
    {% assign highlight_internet = post %}
  {% endif %}
{% endfor %}

<div class="home-page">
  <section class="home-row home-hero">
    <div class="home-hero__content">
      <h1 class="home-hero__title">Yan Luo (&#32599;&#39068;)</h1>
      <div class="home-hero__summary">
        <p>Tenure-track Assistant Professor</p>
        <p><a href="https://www.cityu-dg.edu.cn/en/home">City University of Hong Kong (Dongguan)</a></p>
        <p>Email: luo.yan[AT]my.cityu.edu.hk</p>
        <p>Office: To be confirmed</p>
        <p>
          <a href="https://scholar.google.com/citations?user=W7lRGDQAAAAJ&hl=en">Google Scholar</a>,
          <a href="https://orcid.org/0000-0002-9731-4983">ORCID</a>,
          <a href="https://github.com/YanLuoCityU">GitHub</a>,
          <a href="https://www.linkedin.com/in/yan-luo-618925169">LinkedIn</a>
        </p>
      </div>
    </div>
    <div class="home-hero__media">
      <img
        src="{{ group_photo }}"
        alt="Yan Luo"
        width="6048"
        height="4668"
        style="width: min(100%, 520px); height: auto; max-height: none; object-fit: contain;"
      >
    </div>
  </section>

  <section class="home-row">
    <h2 class="home-section-title">Recent News</h2>
    <ul class="home-news-list{% if recent_news_total_count > 10 %} scrollable{% endif %}">
      {% for item in recent_news limit:5 %}
        <li>
          <span><strong>{{ item.date | date: "%b %d, %Y" }}</strong>: One paper is published in {% if item.url %}<a href="{{ item.url }}"><strong><em>{{ item.journal }}</em></strong></a>{% else %}<strong><em>{{ item.journal }}</em></strong>{% endif %}.</span>
        </li>
      {% endfor %}
      {% for post in sorted_publications %}
        {% assign post_date = post.date | date: "%Y-%m-%d" %}
        {% assign post_paperurl = post.paperurl | default: "" | downcase %}
        {% unless manual_news_urls contains post_paperurl %}
        {% unless post_date == '2021-12-17' and post.venue == 'Innovation in Aging' %}
        {% if post.category == 'publication' and post.date and post_date != '1900-01-01' and post.authors %}
          {% assign publication_authors = post.authors | strip_html | split: ',' %}
          {% assign show_publication = false %}
          {% for raw_author in publication_authors limit: 2 %}
            {% assign candidate_author = raw_author | strip | downcase %}
            {% for alias in author_aliases %}
              {% if candidate_author == alias %}
                {% assign show_publication = true %}
              {% endif %}
            {% endfor %}
          {% endfor %}

          {% if show_publication %}
            <li>
              <span><strong>{{ post.date | date: "%b %d, %Y" }}</strong>: One paper is published in {% if post.paperurl %}<a href="{{ post.paperurl }}"><strong><em>{{ post.venue }}</em></strong></a>{% else %}<strong><em>{{ post.venue }}</em></strong>{% endif %}.</span>
            </li>
          {% endif %}
        {% endif %}
        {% endunless %}
        {% endunless %}
      {% endfor %}
    </ul>
  </section>

  <section class="home-row">
    <h2 class="home-section-title">About Me</h2>
    <div class="home-profile-text">
      <p>Yan Luo is a tenure-track Assistant Professor at <a href="https://www.cityu-dg.edu.cn/en/home">City University of Hong Kong (Dongguan)</a>. His research lies at the intersection of data science and epidemiology, with a focus on leveraging large-scale biobanks, cross-national aging cohorts, and real-world electronic health records to address population health challenges. He is particularly interested in developing AI-driven multiomics models for personalized disease risk prediction, uncovering the multi-dimensional determinants of healthy longevity, and characterizing the dynamic trajectories of aging.</p>

      <p>He received his PhD in Data Science from City University of Hong Kong in 2026 under the supervision of <a href="https://datascience.hku.hk/people/qingpeng-zhang/">Prof Qingpeng Zhang</a>, MSc in Epidemiology and Health Statistics from Peking University in 2022 under the supervision of <a href="https://medic.bjmu.edu.cn/jyjx/szll/index.htm">Prof Beibei Xu</a>, and BM in Preventive Medicine from Peking University in 2020.</p>
    </div>
  </section>

  <section class="home-row">
    <h2 class="home-section-title">Research Interests</h2>
    <ul class="home-compact-list home-interest-list">
      <li>Machine learning</li>
      <li>Deep learning</li>
      <li>Social determinants of health</li>
      <li>Multiomics analysis</li>
      <li>Healthy aging</li>
    </ul>
  </section>

  <section class="home-row">
    <h2 class="home-section-title">Honors &amp; Awards</h2>
    <ul class="home-compact-list home-awards-list">
      <li>IAFOR Scholarship Recipient, The International Academic Forum, 2024</li>
      <li>Award for Academic Excellence, Peking University, 2019-2020</li>
      <li>Award for Contribution in Student Organizations, Peking University, 2018-2019</li>
      <li>The Chugai Scholarship for Excellent Medical Students, Peking University, 2018-2019</li>
      <li>Award for Contribution in Student Organizations, Peking University, 2017-2018</li>
      <li>The Second Prize Scholarship for Excellent Medical Students (The Eisai [China] Pharmaceutical Scholarship), Peking University, 2017-2018</li>
    </ul>
  </section>
  <section class="home-row">
    <h2 class="home-section-title">Selected Publications</h2>
    <div class="research-highlights-grid">
      {% if highlight_multiomics %}
      <section class="research-highlight-card">
        <div class="research-highlight-top">
          <span class="research-highlight-badge">{{ highlight_multiomics.venue }}</span>
        </div>
        <h3><a href="{{ highlight_multiomics.paperurl }}">{{ highlight_multiomics.title }}</a></h3>
        <p class="research-highlight-meta">{{ highlight_multiomics.date | date: "%Y" }}</p>
        <p class="research-highlight-links">
          <a class="research-highlight-link" href="{{ highlight_multiomics.paperurl }}">Paper</a>
          <a class="research-highlight-tag" href="{{ research_highlights_media_url }}#multiomics-cvd">News / Media Coverage</a>
        </p>
      </section>
      {% endif %}

      {% if highlight_internet %}
      <section class="research-highlight-card">
        <div class="research-highlight-top">
          <span class="research-highlight-badge">{{ highlight_internet.venue }}</span>
        </div>
        <h3><a href="{{ highlight_internet.paperurl }}">{{ highlight_internet.title }}</a></h3>
        <p class="research-highlight-meta">{{ highlight_internet.date | date: "%Y" }}</p>
        <p class="research-highlight-links">
          <a class="research-highlight-link" href="{{ highlight_internet.paperurl }}">Paper</a>
          <a class="research-highlight-tag" href="{{ research_highlights_media_url }}#internet-mental-health">News / Media Coverage</a>
        </p>
      </section>
      {% endif %}
    </div>
  </section>
</div>
