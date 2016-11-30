---
layout: post
title: Use this jekyll theme!
date:   2016-11-29 10:50:57 +0800
categories:
  - jekyll
  - rubygem
tags:
  - jekyll theme
---

## Overview

There are many themes provided by jekyll official site, also many open source themes from others.

Here is the theme which I created, I will give the usage for everyone who like it and want to use or re-design it.

This theme is implemented from [bootstrap 3](http://getbootstrap.com/), it's responsive on every platforms.

## Installation

Add this line to your Jekyll site's Gemfile:

{% highlight ruby %}
gem "zslucky"
{% endhighlight %}

And add this line to your Jekyll site's `_config.yml`:

```yaml
theme: zslucky
```

And then execute:

```bash
bundle
```

Or install it yourself as:

```bash
gem install zslucky
```

## Usage

Only need to add follow settings in `_config` yaml file:

```yaml
# username is your name
username: Lucky Zhou
# your avator from assets
avator_url: /assets/img/avator.jpg
# your job title
jobtitle: Front-end Engineer.
# your qq number
qq: 444950680
# your weixin number
weixin: Zsney88
# your weibo number
weibo: zsney88
# your facebook id
facebook: zsney.lzhou
# yourselfs short description
user_desc: Like open source projects, like sharing.
```