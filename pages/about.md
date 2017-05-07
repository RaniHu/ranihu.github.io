---
layout: page
title: About
description: 一名前端程序媛
keywords: ranihu，胡冉,Huran
comments: true
menu: 关于
permalink: /about/
---

我是胡冉，一名前端程序媛。

仰慕「优雅编码的艺术」。

坚信熟能生巧，努力改变人生。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
