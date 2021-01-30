---
layout: page
title: "All posts"
---
<table class="no-border no-background">
    <colgroup>
       <col style="width: 7rem;">
       <col>
    </colgroup>
{% for post in site.posts %}
    <tr>
        <td style="text-align: right;">{{ post.date | date_to_string }}</td>
        <td><a href="{{ post.url }}">{{ post.title }}</a></td>
    </tr>
{% endfor %}
</table>
