---
title: "学习开源仓库BibiGPT"
subtitle: "看下别人的技术栈"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/home-bg-f.jpg"
tags:
  - web
  - product
  - chatgpt
---

> 通过学习开源仓库，了解当下的技术栈，潮流趋势。文章只是作为个人的学习笔记，所以不具备很好的可读性。旨在将来哪天个人查阅

有一天，一个朋友给我推荐一个对视频作总结的工具。最初我以为很了不起，ChatGPT能总结视频了。于是我安装了推荐的插件，插件给了免费试用的几次机会。搜索引擎结果总结、视频内容总结都做得很出色。但是我发现一个问题，如果一个视频没有字幕，它是没法子工作的。

于是我有个推断，这些总结内容的插件是利用文字的总结能力，而非视频或者音频的真实内容。

最近我研究了当时挺火的一个仓库(https://github.com/JimmyLv/BibiGPT-v1)。先说我认为的缺点，代码结构混乱，可读性差，页面排版布局一个乱，商业性太强，创新性不够。

我会总结这个项目里用到的一些技术、资源，可以拿来用的。不需要去通读它的代码，写的真的渣。

先说它的工作流程，

```
视频地址=》解析videoId、视频信息=》savesubs或者b站接口获取字幕=》openAI completions接口总结
```

#### [savesubs](https://savesubs.com/zh)

这是一家免费的字幕获取网站，支持国外主流的视频网站地址。它的cookie **cf_clearance** 可以从浏览器获取,接口地址

```
https://savesubs.com/action/extract
```

#### b站

官方是提供字幕获取接口的，cookie ****SESSDATA** ,字幕获取涉及接口

```
https://api.bilibili.com/x/web-interface/view${params}
https://api.bilibili.com/x/player/v2?aid=${aid}&cid=${cid}
```

#### [upstash]([https://upstash.com](https://upstash.com/))

一家国外的提供serverless服务的公司，提供redis、kafka、vector。redis提供256MB内存，每月50GB流量免费额度。对于小项目开发够用。redis提供了限流API。另外还提供rest api。比较小而美

#### [supabase](https://supabase.com/)

BASS(Backend As a Service)，我第一次听到这个名词，颇有一种Every can be a Service的感觉。这是Firebase的开源替代品，提供后端常用的数据库、认证授权、存储、边缘函数接口。提供各种语言的SDK，支持Docker快速部署。对于规模不大的公司而言，使用它能快速开发，省去很多重复的工作。

#### sentry

项目上线出bug了，测试没测出来，日志不好排查。这是一个错误跟踪统计工具，支持各种语言，比较老牌了。

#### nextjs+tailwind

tailwind这种高度可定制的css框架要求使用的人有创新性，同时开发速度不够快。我会采用radix/ui或者shadcn/ui这种采用tailwind为底层包装过的UI库，我没啥美感，配色、配风格太难了。

nextjs的高度灵活性在国外很流行，支持边缘函数。

#### edge function

边缘函数和边缘计算同时出来的，以前的CDN只能做静态资源管理。如今给边缘加上了计算能力，让某些需要动态计算的接口离用户更近。这算是一种进步。以前动态计算是要访问服务器的，如今的serverless服务大行其道，边缘函数实施起来就没多少难度。

#### 产品

这个开源仓库如今没更新了，它的网站却是另一番景象。用户可以购买视频时长来使用它的服务，60分钟、600分钟。它的网站依然放了很多广告性质的内容。从技术上来讲，一个视频无论多长，获取它的字幕文件，调用openAI的接口只需要一次，只是生成的token会变长。视频越长生成的token越长，费用越多。

让搞技术的来看，颇有割韭菜的感觉。这种技术到产品的风格转换却是值得学习的，如何让一次接口的调用变得有模有样。

它使用的接口除了openAI要收费，其他都是免费的。如果拿来变现，商业上是否存在不道德，后面人家发现了，提高接口调用难度，它的服务岂不是不能用？

AI的产品很多，至今没有一款深入人心的产品。快不是市场需要的，市场要的是价值。

