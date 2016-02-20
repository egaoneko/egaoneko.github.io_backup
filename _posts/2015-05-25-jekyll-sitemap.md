---
layout: post
title: '플러그인 설치 없이 Jekyll에서 Sitemap.xml 생성하기'
date: 2015-05-25 02:50:00
categories: Blog
tags: [Blog, Jekyll, Sitemap.xml]
published: true
fullview: false
comments: true
---

운영하고있는 Jekyll블로그의 Sitemap.xml을 구글 웹마스터에 제출하고자 방법을 찾아보았다. 몇몇 방법들 있었지만 플러그인을 설치하지 않고 Sitemap.xml을 얻어 내고 싶었다.

해결책은 해외 블로그에서 찾을 수 있었다. 원문은 이 [링크](http://davidensinger.com/2013/11/building-a-better-sitemap-xml-with-jekyll/)를 참조하기 바란다.

## 설정 방법

### 블로그 Root폴더에 sitemap.xml파일을 생성하고 다음 내용을 작성한다.

```xml
---
layout: null
sitemap:
  exclude: 'yes'
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    {% unless post.published == false %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.sitemap.lastmod %}
        <lastmod>{{ post.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
      {% elsif post.date %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
      {% endif %}
      {% if post.sitemap.changefreq %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% else %}
        <changefreq>monthly</changefreq>
      {% endif %}
      {% if post.sitemap.priority %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% else %}
        <priority>0.5</priority>
      {% endif %}
    </url>
    {% endunless %}
  {% endfor %}
  {% for page in site.pages %}
    {% unless page.sitemap.exclude == "yes" %}
    <url>
      <loc>{{ site.url }}{{ page.url | remove: "index.html" }}</loc>
      {% if page.sitemap.lastmod %}
        <lastmod>{{ page.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
      {% elsif page.date %}
        <lastmod>{{ page.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
      {% endif %}
      {% if page.sitemap.changefreq %}
        <changefreq>{{ page.sitemap.changefreq }}</changefreq>
      {% else %}
        <changefreq>monthly</changefreq>
      {% endif %}
      {% if page.sitemap.priority %}
        <priority>{{ page.sitemap.priority }}</priority>
      {% else %}
        <priority>0.3</priority>
      {% endif %}
    </url>
    {% endunless %}
  {% endfor %}
</urlset>
```

### _config.yml에 다음 내용을 추가한다.

```YAML
sitemap:
  lastmod: 2014-01-23
  priority: 0.7
  changefreq: 'monthly'
  exclude: 'yes'
```


## 추가 설정 방법

필자가 상단의 설정을 하고 블로그를 실행시켰을 경우 sitemap.xml에 생성되는 것은 확인되었으나, 필자의 경우 github를 통해 호스팅하여 ``site.url``을 설정해두지 않아 아래와 같이 sitemap.xml이 제대로 생성되지 않았다. 그래서 필자는 위와 다르게 설정을 다음과 같이 조금 변경해보았다.

```xml
<url>
      <loc>/blog/2015/05/25/jekyll-sitemap.html</loc>
      <lastmod>2015-05-25T11:50:00+09:00</lastmod>
      <changefreq>monthly</changefreq>
      <priority>0.5</priority>
</url>
```

### 블로그 Root폴더에 sitemap.xml파일을 생성하고 다음 내용을 작성한다.

```xml
---
layout: null
sitemap:
  exclude: 'yes'
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    {% unless post.published == false %}
    <url>
      {% if site.url %}
        <loc>{{ site.url }}{{ post.url }}</loc>
      {% else %}
        <loc>{{ site.sitemap.url }}{{ post.url }}</loc>
      {% endif %}
      {% if post.sitemap.lastmod %}
        <lastmod>{{ post.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
      {% elsif post.date %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
      {% endif %}
      {% if post.sitemap.changefreq %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% else %}
        <changefreq>monthly</changefreq>
      {% endif %}
      {% if post.sitemap.priority %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% else %}
        <priority>0.5</priority>
      {% endif %}
    </url>
    {% endunless %}
  {% endfor %}
  {% for page in site.pages %}
    {% unless page.sitemap.exclude == "yes" %}
    <url>
      {% if site.url %}
        <loc>{{ site.url }}{{ post.url }}</loc>
      {% else %}
        <loc>{{ site.sitemap.url }}{{ page.url | remove: "index.html" }}</loc>
      {% endif %}
      {% if page.sitemap.lastmod %}
        <lastmod>{{ page.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
      {% elsif page.date %}
        <lastmod>{{ page.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
      {% endif %}
      {% if page.sitemap.changefreq %}
        <changefreq>{{ page.sitemap.changefreq }}</changefreq>
      {% else %}
        <changefreq>monthly</changefreq>
      {% endif %}
      {% if page.sitemap.priority %}
        <priority>{{ page.sitemap.priority }}</priority>
      {% else %}
        <priority>0.3</priority>
      {% endif %}
    </url>
    {% endunless %}
  {% endfor %}
</urlset>
```

### _config.yml에 다음 내용을 추가한다.

```YAML
sitemap:
  lastmod: 2014-01-23
  priority: 0.7
  changefreq: 'monthly'
  url: 'github 주소'
  exclude: 'yes'
```

상단과 같이 설정할 경우 아래와 같이 정상적으로 sitemap.xml이 생성되는 것을 확인할 수 있다.

```xml
<url>
      <loc>http://egaoneko.github.io/blog/2015/05/25/jekyll-sitemap.html</loc>
      <lastmod>2015-05-25T11:50:00+09:00</lastmod>
      <changefreq>monthly</changefreq>
      <priority>0.5</priority>
</url>
```
