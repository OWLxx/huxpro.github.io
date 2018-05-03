---
layout:     post
title:      "Jekyll Notes"
date:       2018-04-12 12:00:00
author:     "Xin"
tags:
    - Web

---

## 1. Documents

### 1.1 Create blog
> jekyll new NAME_OF_BLOG

### 1.2 Run the website locally
> bundle exec jekyll serve
>
> bundle install

### 1.3 folders
_posts: blogs <br>
_site: final output of website <br>
_config.yml:  setting for jekyll website like title, email, description... <br>
Gemfile: save all the dependencies of the website <br>

## 2. Front matter
Front matter can be written by YAML or JSON
example
```
---
layout: post
title:  "Welcome to Jekyll!"
date:   2018-04-11 19:14:09 -0400
categories: jekyll update
author: OWL
---
```
every page need a front matter

## 3. Theme

The default theme is minima. 

www.rubygems.org  search for jekyll-theme

**bundle install** would install themes in Gemfile

change layout name in the front matter if it's not included

### 3.1 Make own layout

1. Create _layout folder

2. layout should be HTML, like post.html

   ​

   ​






​    
