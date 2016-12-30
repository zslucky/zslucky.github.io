---
layout: post
title: How to use jekyll theme ?!
chineseTitle: 如何使用jekyll主题？！
icon: clone
date:   2016-11-29 10:50:57 +0800
categories:
  - css
  - html
  - javascript
tags:
  - jekyll
---

---

## Overview

There are many themes provided by jekyll's official site, also many open source themes provided by others.

Here is the theme which I created, I will give the usage for everyone who like it and want to use or re-design it.

This theme is implemented from [bootstrap 3](http://getbootstrap.com/), it's responsive on every platforms.

Jekyll官方提供了很多的主题可以选择，当然也有很多个人开源的主题可以使用。

我创建了这个博客站主题，我会提供给他人使用方法或者如何重定制他。

这个主题使用了bootstrap 3，能够自适应各种平台。

---

## Contents

- [**Installation** - 安装](#installation)
- [**Usage** - 使用方法](#usage)
- [**Know issues** - 已知问题](#know-issues)

---

## Installation

Add this line to your Jekyll site's `Gemfile`:

添加以下代码到你的`Gemfile`。

```ruby
gem "zslucky"
```

And add this line to your Jekyll site's `_config.yml`:

然后添加以下配置到你的`_config.yml`文件。

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

---

## Usage


Only need to add follow settings in `_config.yml` yaml file:

只需要添加以下配置到你的`_config.yml`文件。

```yaml
# username is your name - 站主名字
username: Lucky Zhou
# your avator from assets - 头像图片路径
avator_url: /assets/img/avator.jpg
# your job title - 职位名称
jobtitle: Front-end Engineer.
# your qq number - qq号
qq: 444950680
# your weixin number - 微信号
weixin: Zsney88
# your weibo number - 新浪微博号
weibo: zsney88
# your facebook id - facebook号
facebook: zsney.lzhou
# yourselfs short description - 个人简述
user_desc: Like open source projects, like sharing.
```

---

## Know issues


Deploy to Github Page using auto build will cause build failed due to theme unsupported. I found 2 solutions here:

1. Using `jekyll build` to build static files, then only publish all files in `_site` directory.
2. Clone this theme repo, copy it to your jekyll site.

使用自动部署到Github Page会出现不支持主题的错误，这里我有2种解决方案：

1. 使用 `jekyll build` 生成静态网站，然后仅仅只需部署 `_site` 目录下的文件即可。

2. 克隆我的主题仓库到你文件下直接使用。
