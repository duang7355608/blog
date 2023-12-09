---
layout: "page"
title: "Collections"
permalink: "/collections/"
---
<!--md-->

<div>
    {% for collection in site.collections %}
        <h2>
            <a  href="{{ site.baseurl }}/{{ collection.label | escape }}/">
                 {{ collection.label | escape }}
            </a>
        </h2>
    {% endfor %}
</div>
<!--md-->