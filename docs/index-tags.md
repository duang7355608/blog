---
layout: "page"
title: "Tags"
permalink: "/tags/"
---
<!--md-->
<div>
{% assign all_tags = "" | split: "" %}

{% for post in site.documents %}

{% assign post_tags = post.tags %}

{% assign all_tags = all_tags | concat: post_tags | uniq %}

{% endfor %}
</div>

<div>
{% for tag in all_tags %}
    <h3>{{ tag | escape }}
        <a hidden="hidden" href="{{ site.baseurl }}/{{ tag | escape }}/">
            {{ tag | escape }}
        </a>
    </h3>
    {% for post in site.documents %}
        {% if post.tags contains tag %}
            <ul>
               <li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
            </ul>
        {% endif %}
    {% endfor %}    
{% endfor %}
</div>
<!--md-->
