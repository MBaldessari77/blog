---
---

I'm a software developer from more than 20 years... my passion? **write code :)**

# My posts

<ul>
  {% for post in site.posts %}
    <li>
      {{ post.date }} <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

To see my posts written before 2017 visit my <a href="http://blogs.ugidotnet.org/mb" target="_blank">italian blog</a>
