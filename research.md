---
---

<h1>Research</h1>

{% assign all_research = site.research | sort: 'date' | reverse %}

{% if all_research.size > 0 %}
<section class="pan-post-list">
  {% for item in all_research %}
  <div class="pan-post-list__item">
    <div class="pan-post-list--left">
      <div class="pan-post-list--image">
        {% if item.image %}
        <a href="{{ item.url | relative_url }}" style="background-image: url({{ item.image | relative_url }});"></a>
        {% else %}
        <a href="{{ item.url | relative_url }}"></a>
        {% endif %}
      </div>
    </div>
    <div class="pan-post-list--right">
      <div class="pan-post-list--content">
        <div class="pan-post-list--body">
          <h2><a href="{{ item.url | relative_url }}">{{ item.title }}</a></h2>
          {% if item.date %}
          <p class="info">
            <span>{{ item.date | date: '%Y, %b %d' }}</span>
          </p>
          {% endif %}
          <p class="desc">{{ item.content | strip_html | truncatewords: 20 }}</p>
        </div>
      </div>
    </div>
  </div>
  {% endfor %}
</section>
{% else %}
<p>내용은 추후 작성 예정입니다.</p>
{% endif %}
