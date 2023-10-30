---
title: "How To Become a Better Engineer"
layout: splash
permalink: /
date: 2016-03-23T11:48:41-04:00
entries_layout: grid
header:
  overlay_color: "#000"
  overlay_filter: "0.2"
  overlay_image: assets/images/blue-purple-abstract.jpg
  actions:
    - label: "Read About It"
      url: "/How-to-get-better"
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
excerpt: "The final key is to deliver. When learning these tools of the trade, find a project to use them in. Either become a serious contributor to an open source project or build a tool yourself. Whichever option you choose, make sure you do it with care."
intro: 
  - excerpt: 'I am a Sr. Software Engineer with 8 years of professional experience. I am well versed in the JavaScript ecosystem with expertise in Node.js and Vue.js. I am also an expert Ruby on Rails developer. In my time as an engineer, I have worked with tools such as Elasticsearch, MongoDB, PostgreSQL, Nightwatch, mocha, RSpec, Redis, AWS S3, Jenkins and many other web and internetworking technologies'
feature_row:
  - image_path: assets/images/maintenance-sprint.png
    alt: "placeholder image 1"
    title: "Placeholder 1"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
  - image_path: /assets/images/collaborate.png
    image_caption: "Image courtesy of [Unsplash](https://unsplash.com/)"
    alt: "placeholder image 2"
    title: "Placeholder 2"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
  - image_path: /assets/images/tool-bag.png
    title: "Placeholder 3"
    excerpt: "This is some sample content that goes here with **Markdown** formatting."
feature_row2:
  - image_path: /assets/images/development.png
    alt: "placeholder image 2"
    title: "Placeholder Image Left Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Left aligned with `type="left"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row3:
  - image_path: /assets/images/padlock.png
    alt: "placeholder image 2"
    title: "Placeholder Image Right Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Right aligned with `type="right"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row4:
  - image_path: /assets/images/strategy.png
    alt: "placeholder image 2"
    title: "Placeholder Image Right Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Right aligned with `type="right"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
feature_row5:
  - image_path: /assets/images/journey-man-image.png
    alt: "placeholder image 2"
    title: "Placeholder Image Center Aligned"
    excerpt: 'This is some sample content that goes here with **Markdown** formatting. Centered with `type="center"`'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"
---

![image-left]({{ site.url }}{{ site.baseurl }}/assets/images/kyle.jpg){: .align-left} {% include feature_row id="intro" type="center" %}

{% include feature_row %}

{% for post in site.posts limit: 12 %}
  {% include archive-single.html type="grid" %}
{% endfor %}

{% include feature_row id="feature_row5" type="center" %}