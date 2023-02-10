# About this Cool Concise Jekyll Blog Theme

## Content

* [Page Details](#page-details)
    * [Home](#home)
    * [Archives](#archives)
    * [Categories](#categories)
    * [Tags](#tags)
    * [Collections](#collections)
    * [Demo](#demo)
    * [About](#about)
    * [Comments](#comments)
    * [Statistical Analysis](#statistical-analysis)
* [Usage](#usage)
    * [1. Install ruby and jekyll environment](#1-install-ruby-and-jekyll-environment)
    * [2. Copy theme code](#2-copy-theme-code)
    * [3. Change parameter](#3-change-parameter)
        * [Basic info](#basic-info)
        * [Link info](#link-info)
        * [Comments info](#comments-info)
        * [Statistical analysis info](#statistical-analysis-info)
    * [4. Write post](#4-write-post)
    * [5. Local launch](#5-local-launch)
    * [6. Push to GitHub](#6-push-to-github)


## Page Details

### Home

Index page show 5 posts excerpt as a default. Readers can click article title or read more button to see full post. There are recent posts area, categories area and tags area at the right part of the index page. You can also add an area at this part, if you change the file `index.html`.

### Archives

Archive post according to the year.

### Categories

Show posts according to the category.

### Tags

Show posts according to the tags.

### Collections

The user can collect their favorite article links with `markdown` syntax.

### Demo

I use *[Masonry](http://masonry.desandro.com/)* to rewrite the waterfall responsive layout. Better interactive experience.

### About

The user can write some introduction about theirselves and their site with `markdown` syntax.

### Comments

This theme supports both [disqus](https://disqus.com/) , [多说评论](http://duoshuo.com/) , [来必力](https://www.livere.com/) and [畅言云评](https://changyan.kuaizhan.com/). It's very easy to config your comments module.

The only thing you need do is to change the `short_name` in the file `_config.yml`. As follows.

```yml
# comments
# four ways to comment, if you want to use one of the first two, fill in your own short name; 
# if you want to use one of the last two, just fill in any value, but cannot be empty.
# 四种评论插件，如果选前两个中的一个，填写自己的 short_name；如果选后两个中的一个，填写任意值
duoshuo_shortname: #nora_nan
disqus_shortname: #nora_nan
livere_flag: #标识是否开启来必力评论
changyan_flag: True    #标识是否开启畅言评论
```

### Statistical Analysis

This theme supports Google Analytics and Baidu Statistics， you can just config the id in the file `_config.yml`, as follows.

```yml
# statistic analysis 统计代码
# 百度统计 id，将统计代码替换为自己的百度统计id，即
# hm.src = "//hm.baidu.com/hm.js?xxxxxxxxxxxx";
# xxxxx字符串
baidu_tongji_id: xxxxxxxxxxxx
google_analytics_id: UA-xxxxxxxx # google 分析追踪id
```

## Usage

Welcome everyone to use this theme, this part shows introduction to use.

### 1. Install ruby and jekyll environment

This step and Step 5 mainly talk to you how to launch blog at local. If you don't want to launch at local, you can ignore these 2 steps. But I still strongly suggest to do this. Ensure there is nothing wrong before pushing to the github.

The Windows users can directly use [RubyInstaller](http://rubyinstaller.org/) to install ruby environment. Follow the prompts while installing.

Install jekyll commands:

```
gem install jekyll
```

For more details, you can view the jekyll official website. [https://jekyllrb.com/](https://jekyllrb.com/)

There may be something wrong at mac OS X El Capitan, you can see the solution at [https://jekyllrb.com/docs/troubleshooting/#jekyll-amp-mac-os-x-1011]( https://jekyllrb.com/docs/troubleshooting/#jekyll-amp-mac-os-x-1011).

If you are interesting in jekyll, you can see the jekyll source code at [https://github.com/jekyll/jekyll](https://github.com/jekyll/jekyll).

![jekyll logo](http://jekyllcn.com/img/logo-2x.png)

### 2. Copy theme code

You can clone, download or fork this repo.

### 3. Change parameter

Mainly change the parameters at file `_config.yml` and use your own `favicon.ico`.

#### Basic info

Shows at site header part.

```yml
# Site settings
title: NMT
brief-intro: Data Mining & Big Data
baseurl: "" # the subpath of your site, e.g. /blog
url: "https://norah2.github.io/" # the base hostname & protocol for your site
```

#### Link info

Mainly shows at the footer of the site.

```yml
# other links
# twitter_username:
# facebook_username: 
github_username:  norah2
email: 2238708584@qq.com
# weibo_username: 
# zhihu_username: 
# linkedIn_username: 
# dribbble_username:

description_footer: 当你的才华撑不起你的野心时，只有静下心来好好学习！
```

#### Comments info

Get your own `short_name`:

Visit https://disqus.com/ , http://duoshuo.com/ , https://www.livere.com/ or https://changyan.kuaizhan.com/ . And follow the prompts at the site.

```yml
# comments
# four ways to comment, if you want to use one of the first two, fill in your own short name; 
# if you want to use one of the last two, just fill in any value, but cannot be empty.
# 四种评论插件，如果选前两个中的一个，填写自己的 short_name；如果选后两个中的一个，填写任意值
duoshuo_shortname: #nora_nan
disqus_shortname: #nora_nan
livere_flag: #标识是否开启来必力评论
changyan_flag: True    #标识是否开启畅言评论
```

When you done, you can also see the comments info at disqus or duoshuo admin console.

#### Statistical analysis info

Get Google Analytics id or Baidu Statistics id：

Visit https://www.google.com/analytics/ or http://tongji.baidu.com/. And follow the prompts at the site.

Of course, if you don't want any statistical and analysis info, you can type nothing at id position.

```yml
# statistic analysis 统计代码
# 百度统计 id，将统计代码替换为自己的百度统计id，即
# hm.src = "//hm.baidu.com/hm.js?xxxxxxxxxxxx";
# xxxxx字符串
baidu_tongji_id: cf850xxxxxxxxxxxxxxxx
google_analytics_id: UA-7xxxxxx-4 # google 分析追踪id
```

When you done, you can see UV, PV, location etc. info at your own Google Analytics or Baidu Statistic console.

### 4. Write post

You can write posts at folder `_posts`. At the beginning of the post, you should declare layout、title、date、categories、tags、author(optional) info、mathjax(optional，click [here](https://www.mathjax.org/) for more detail about `Mathjax`).

```
---
layout: post
title:  
date: 
categories: 
tags: 
author: 
mathjax: true
---
```

These follow code is for making contents.
```
* content
{:toc}
```

You can use 4 wraps as a excerpt separator. The words before separator as excerpt show in the index page. When you enter the post page, you can read full article.

The wraps config is in the file `_config.yml`, as follows:

```yml
# excerpt
excerpt_separator: "\n\n\n\n"
```

You should use markdown syntax to write article, just like write readme in github.

You can use 3 \`\`\` to write code block.

### 5. Local launch

use command:

```
jekyll s
```

Terminal shows:

```
Configuration file: E:/GitWorkSpace/blog/_config.yml
            Source: E:/GitWorkSpace/blog
       Destination: E:/GitWorkSpace/blog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 6.33 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'E:/GitWorkSpace/blog'
Configuration file: E:/GitWorkSpace/blog/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

Visit localhost:4000 to see your blog!!!

### 6. Push to GitHub

If there is nothing wrong, push code to your github!

