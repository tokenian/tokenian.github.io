---
title: "论数学对编程的影响（三）"
subtitle: "再谈形式主义"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/post-bg-infinity.jpg"
tags:
  - life
  - math
  - philosophy
---

> 形式主义这个学派是由德国数学家大卫-希尔伯特（1862-1943）于1910年左右创立的。用一些符号来构建一个无矛盾的系统，意图使得在这个系统里的定理、公式都是正确的。形式主义的大多数学家或者哲学家思想认为数学对象不是独立存在的，无穷总体不具有客观性，它是一种有用的虚构。数学不是数学家发现的，而是发明的，好像音乐那样，是思想的创造物，是一种无意义的符号游戏。

我们现在运行的电脑、手机，各种电子设备，它们的硬件架构是非常类似的。早期的计算机采用的是冯-诺伊曼结构，是一种将程序指令存储器和数据存储器合并在一起的电脑设计概念结构。这种结构的设计者冯-诺伊曼，年轻时师承于大卫-希尔伯特。

编程语言跑在各种机器上（电脑、手机、各种嵌入式设备），执行的是二进制机器码指令，就像血液一样在内部流淌。同样的代码可以同时运行在不同的设备，小米手机、苹果手机、电脑模拟器。这些设备底层的硬件架构是相似的，编译器把代码变成对应平台适配的指令。门派可以不同，祖师却是同一个。

通俗的理解：女生喜欢买东西，衣柜总是堆满了各种衣服。每天轮流穿都不知道要换到何年何月，形式那么多，人就那么一个！

```bash
curl 'https://www.baidu.com/' \
  -H 'Connection: keep-alive' \
  -H 'Cache-Control: max-age=0' \
  -H 'sec-ch-ua: " Not;A Brand";v="99", "Google Chrome";v="91", "Chromium";v="91"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'DNT: 1' \
  -H 'Upgrade-Insecure-Requests: 1' \
  -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36' \
  -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9' \
  -H 'Sec-Fetch-Site: none' \
  -H 'Sec-Fetch-Mode: navigate' \
  -H 'Sec-Fetch-User: ?1' \
  -H 'Sec-Fetch-Dest: document' \
  -H 'Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,zh-TW;q=0.7' \
  -H 'Cookie: BIDUPSID=5D836150D9783A118C3CDFC3A8136E4D; PSTM=1615780479; BD_UPN=12314753; BDUSS=HVpVE5CQ1JJR2RweVJSM0Rad3BOMERDbTBNWTh3V09WOTNqV3NINklXZTBuM1pnRVFBQUFBJCQAAAAAAAAAAAEAAACApeoKdG9rZW5pYW4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAALQST2C0Ek9gc; BDUSS_BFESS=HVpVE5CQ1JJR2RweVJSM0Rad3BOMERDbTBNWTh3V09WOTNqV3NINklXZTBuM1pnRVFBQUFBJCQAAAAAAAAAAAEAAACApeoKdG9rZW5pYW4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAALQST2C0Ek9gc; BAIDUID=C9E93DC2F0B990A50B5A15716C963F9A:FG=1;' \
  --compressed
```

我们看看通过命令`curl`获取百度主页的命令，首先跟着的是百度的网页地址，然后是`HTTP Header`，每个请求头前面都是`-H`。`--comressed`这个参数告诉服务器，客户端接收压缩数据。这个curl命令的使用看起来很简洁，直观，具有非常好的形式。

主命令`curl`，各种不同的参数，参数后面紧跟一个空格，然后是具体参数值。使用者很容易学习、掌握这种方式，而大多数的`Linux`命令和这个类似。

形式主义所表达出的，不论是语言（`css、HTML、SQL`），还是命令（`ls top netstat`），或者跳出计算机范畴的范式，都是很易于我们学习的。我们也很容易构建这样的一种封闭的小系统来解决现实问题。

那形式主义达到了它们的目的了吗？就是系统内的定理都能被证明，彼此无矛盾。没有！

