---
layout: "page"
title: "Tags"
permalink: "/tags/"
---
<!--md-->
<div>
    {% for staff_member in site.staff_members %}
        <h2>
            <a href="{{ staff_member.url }}">
                {{ staff_member.name }} - {{ staff_member.position }}
            </a>
        </h2>
        <p>{{ staff_member.content | markdownify }}</p>
    {% endfor %}
</div>

<div>
    {% for tag in site.tags %}
        <h3>{{ tag[0] }}</h3>
        <ul>
            {% for post in tag[1] %}
                <li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
            {% endfor %}
        </ul>
    {% endfor %}
</div>

<!--md-->