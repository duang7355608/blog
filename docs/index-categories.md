---
layout: "page"
title: "Categories"
permalink: "/categories/"
---
<!--md-->

<div>
    {% for category in site.categories %}
        <h3>{{ category[0] }}</h3>
        <ul>
            {% for post in category[1] %}
                <li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
            {% endfor %}
        </ul>
    {% endfor %}
</div>

<!--md-->