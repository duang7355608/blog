---
layout: "post"
title:  "Jekyll-Collections"
date:   "2023-12-09 00:00:00 +0800"
categories: [jekyll]
tags: [jekyll]
modified_date:   "2023-12-10 00:00:00 +0800"
---
{% include /custom/toc.md %}

> **Jekyll官网-** [jekyllrb.com](https://jekyllrb.com/)  
> [jekyll-Collections](https://jekyllrb.com/docs/collections/)  
> [jekyll-Configuration](https://jekyllrb.com/docs/configuration/)  
> [jekyll-Variables](https://jekyllrb.com/docs/variables/)

## tags/categories/collections

- jekyll 有 tags/categories/collections 三个功能可以用来做分类标识整理。启用自定义collection后影响比较大。

> `site.tags`和`site.categories`只会收集`_config.yml`中`collections_dir`根目录下`_posts`目录里面的信息。
> #### 注意！！！如果启用了自定义collection目录，自定义的其他collection目录内文章不会在这里俩变量中生效。
> `collections_dir`默认是`"."`,也可以调整为其他,要注意同时转移`_posts`和`_drafts`目录到新`collections_dir`目录下。

### tags

tags最简单，在Front Matter中可以给一篇文章标记多个tag，仅仅是数据标记，没什么其他影响。

- 可以用{% raw %}`{{ post.tags }}`{% endraw %}来查看文章的tag列表。
- 在默认情况下，没有启用自定义collections功能时，`_post`目录就是默认的`collection`目录，系统会自动把`_post`目录下所有文章归类生成一个`site.tags`。
    - 此时可以通过`site.tags`来查看所有tags，`site.tags[tagname]`来查看tagname对应的文章列表。
    - tag列表页常用例子：   
      {% raw %}
      ```liquid
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
       ```

{% endraw %}

- 如果使用了自定义collections功能，`site.tags`也只会有`collections_dir`下`_post`的内容，并不会收录自定义`collection`目录下内容

### categories

categories，在Front Matter中可以给一篇文章标记多个category，除了跟`tags`类似能做数据标记，另外还可能会影响文章的生成路径和访问路径。  
与tags类似：

- 可以用{% raw %}`{{ post.categories }}`{% endraw %}来查看文章的category列表。
    - 在默认情况下，没有启用自定义collections功能时，`_post`目录就是默认的`collection`目录，系统会自动把`_post`目录下所有文章归类生成一个`site.categories`。
        - 此时可以通过`site.categories`来查看所有categories，`site.categories[categoryname]`来查看categoryname对应的文章列表。
        - categories列表页常用例子：  
          {% raw %}
          ```liquid
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
           ```
          {% endraw %}
- 如果使用了自定义collections功能，`site.categories`也只会有`collections_dir`下`_post`的内容，并不会收录自定义`collection`目录下内容

### collections

#### 介绍

- collection相对于tags、categories来说，像是更高一级的区分，也是一个物理路径。文章直接在期望的collection目录下撰写。
- 有一个collection的父路径`collections_dir`，默认是`"."`，`_post`目录就是在默认目录下的一个默认`collection`目录。
- 我们可以在`collections_dir`下继续自定义其他`collection`目录，源文件夹必须是下划线`_`开头,比如：
    - 自定义一个`tech`collection，在`_tech`目录下创建文章，访问路径为{% raw %}`{{ site.baseurl }}/tech/xxx.html`{% endraw %}。
    - 自定义一个`blog`collection，在`_blog`目录下创建文章，访问路径为{% raw %}`{{ site.baseurl }}/blog/xxx.html`{% endraw %}。
    - 自定义一个`project`collection，在`_project`目录下创建文章，访问路径为{% raw %}`{{ site.baseurl }}/project/xxx.html`{% endraw %}。
- `collections_dir`可以自定义，从默认的`.`切换到其他新建目录，比如`my_collections`,如果还会继续用默认生成的`_posts`和`_drafts`目录，也需要一起转移到`my_collections`
  目录下。
- 每个目录可以设置各种不同的layout或其他Front Matter元数据。
- 启用自定义collections后，带来的问题是`site.tags` 和 `site.categories`作用被大幅削弱。

> 说实话，我刚搭好这套系统，还没什么内容，在分类自定义方面，还没体会到多个collections带来的便利，带来的复杂和坑倒是有很多。。
> 是不是直接默认`_post`下建子目录分类的方案就够用。。。

#### 一些改造解决方案

为了能继续达到类似`site.tags` 和 `site.categories`的使用效果。做了一些简单的layout修改。  
思路就是使用不受自定义collection影响的`site.documents`来获取所有文章后多次遍历处理。
> 大坑在于liquid语法不能自由创建和加工一个类似`site.tags`的数据结构，更多的用法是读取和过滤数据，只能通过多次重复遍历来处理。*

##### 博客首页样式

{% raw %}

- 调整前
  ```html
   {% assign posts = site.posts %}
   ```

- 调整后
    ```html
    {% assign posts = site.documents | reverse | where_exp: "post", "post.layout != 'collection-home'" %}
    ```

{% endraw %}

##### tags索引页和categories索引页

以Categories为例： {% raw %}

```html
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
```
{% endraw %}

---

