---
layout: archive
permalink: /machine-learning/
title: "Machine Learning Posts"
author_profile: true
---

{% comment %}
=======================
The following part extracts all the tags from your posts and sort tags, so that you do not need to manually collect your tags to a place.
=======================
{% endcomment %}
{% assign rawtags = "" %}
{% for post in site.posts %}
	{% assign ttags = post.tags | join:'|' | append:'|' %}
	{% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}

{% comment %}
=======================
The following part removes duplicated tags and invalid tags like blank tag.
=======================
{% endcomment %}
{% assign tags = "" %}
{% for tag in rawtags %}
	{% if tag != "" %}
		{% if tags == "" %}
			{% assign tags = tag | split:'|' %}
		{% endif %}
		{% unless tags contains tag %}
			{% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
		{% endunless %}
	{% endif %}
{% endfor %}

{% comment %}
=======================
The purpose of this snippet is to list all the tags you have in your site.
=======================
{% endcomment %}
{% for tag in tags %}
	<a href="#{{ tag | slugify }}"> {{ tag }} </a>
{% endfor %}


{% comment %}
=======================
The purpose of this snippet is to list all your posts posted with a certain tag.
=======================
{% endcomment %}
{% for tag in tags %}
	<h2 id="{{ tag | slugify }}">{{ tag }}</h2>
	<ul>
	 {% for post in site.posts %}
		 {% if post.tags contains tag %}
		 <li>
		 <h3>
		 <a href="{{ post.url }}">
		 {{ post.title }}
		 <small>{{ post.date | date_to_string }}</small>
		 </a>
		 {% for tag in post.tags %}
			 <a class="tag" href="/blog/tag/#{{ tag | slugify }}">{{ tag }}</a>
		 {% endfor %}
		 </h3>
		 </li>
		 {% endif %}
	 {% endfor %}
	</ul>
{% endfor %}
<!---
<ul>
  {% for tag in group_names %}
    {% assign posts = group_items[forloop.index0] %}

    <li>
      <h2>{{ tag }}</h2>
      <ul>
        {% for post in posts %}
        <li>
          <a href='{{ site.baseurl }}{{ post.url }}'>{{ post.title }}</a>
        </li>
        {% endfor %}
      </ul>
    </li>
  {% endfor %}
</ul>

{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
