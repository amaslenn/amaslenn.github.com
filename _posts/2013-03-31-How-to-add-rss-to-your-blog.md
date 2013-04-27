---
title: Как добавить RSS для своего блога
layout: post
tags: [rss, blog, dev]
---

Для этого нужно совсем немного времени и ваш любимый редактор кода.

Сам формат RSS хорошо расписан [тут](http://cyber.law.harvard.edu/rss/rss.html), а вот [тут](https://github.com/snaptortoise/jekyll-rss-feeds) есть реальные примеры для вашего GitHub Page/Jekyll сайта.

    ---
    layout: none
    ---
    <?xml version="1.0" encoding="UTF-8"?>
    <rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
        <channel>
            <title>{ { site.name }}</title>
            <link>{ { site.url }}</link>
            <description>{ { site.description }}</description>
            <atom:link href="{ { site.url }}/feed/index.xml" rel="self" type="application/rss+xml"/>
            { % for post in site.posts % }
            <item>
                <title>{ { post.title }}</title>
                <link>{ { site.url }}{ { post.url }}</link>
                <description>{ { post.content | strip_html | truncatewords: 42 }}</description>
                <pubDate>{ { post.date | date: "%a, %d %b %Y %H:%M:%S %z" }}</pubDate>
                <guid isPermaLink="true">{ { site.url }}{ { post.url }}</guid>
            </item>
            { % endfor %}
        </channel>
    </rss>
Note: чтобы Jekyll не обрабатывал код, я добавил пробелы между { и { и межу { и %.

Сам файлик сохраняем как index.xml и кладем в директорию feed. Теперь, чтобы добавить ваш блог в любой RSS reader нужно будет указать site_url/feed.

Осталось только добавить три новых поля в \_config.yml:

    name       : <site name>
    description: <site description>
    url        : <http://site-url>

**PS** наткнулся на багу, кстати. Если элемент списка начинается с русских символов, то в HTML он списком не будет. [GitHub Issue](https://github.com/mojombo/jekyll/issues/908)  
**PPS** ну и с вставкой кода Liquid, который бы не был обработан, криво получилось. Но работает.
