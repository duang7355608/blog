---
layout: "page"
title: "Categories"
permalink: "/categories/"
---

<!--md-->
<div>
{% assign all_categories = "" | split: "" %}

{% for post in site.documents %}

{% assign post_categories = post.categories %}

{% assign all_categories = all_categories | concat: post_categories | uniq %}

{% endfor %}
</div>

<div>
{% for category in all_categories %}
    <h3>{{ category | escape }}
        <a hidden="hidden" href="{{ site.baseurl }}/{{ category | escape }}/">
            {{ category | escape }}
        </a>
    </h3>
    {% for post in site.documents %}
        {% if post.categories contains category %}
            <ul>
               <li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
            </ul>
        {% endif %}
    {% endfor %}    
{% endfor %}
</div>
<!--md-->
