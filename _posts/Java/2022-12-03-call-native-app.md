---
title: "短信里的链接如何唤起手机上的app"
subtitle: "感兴趣分析下"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/home-bg-f.jpg"
tags:
  - web
---

> 对这个稍微感兴趣，把手机上收到的短信做下分析

### 短信链接

```html
【美团】数字人民币硬钱包限量发放中，你符合免费领取资格，去领取>> dpurl.cn/6bIytq8a 回T退订
```

链接`dpurl.cn/6bIytq8a` 里会做302跳转到 `https://i.meituan.com/c/Nzk5NmRhMWQt?lch=sms_134157_1597819375908728873`,这是美团唤起`app`的`h5`页面

`dpurl.cn` 这个地址会永久定向到大众点评网站

`sms_134157_1597819375908728873`这里定位发送给用户的手机号，账号信息。用户手机唤起之后跳转到哪个页面需要这个信息来判断

### 美团`h5`唤起页面

`html`源码里有很多js链接引用，通过分析定位到

```html
<link href="//awp-assets.meituan.net/mtweb/wind-page/js/meituan.a15d86eb.js" rel="preload" as="script">
```

页面上有两个按钮，一个是下载客户端

```js
downloadApp: function(A) {
    var t = "meituan";
    this.downloadUrl && -1 !== this.downloadUrl.indexOf("/MeituanLite/") && (t = "meituanLite"),
        LXAnalytics("moduleClick", "b_cube_b5dju9nx_mc", {
        pwd: this.key,
        type: t
    }),
    this.writeCode(),
    window.location.href = this.downloadUrl || d
}
```

这里提供下载地址，赋值给浏览器导航栏就行

另一个按钮就是唤起美团`App`

```js
callNative: function() {
    f._isAndroid()) {
    var r = document.createElement("iframe");
    r.src = t,
    r.style.display = "none",
    document.body.appendChild(r)
    } else
    window.location.href = t;
    f._isVivo() ? this.showInfoPop("您当前的浏览器版本可能无法自动唤起") : f._isMobile() && (f._isChrome() || 	   f._isUC()) && this.showInfoPop("您当前的浏览器版本可能无法自动唤起，若未正常唤起，请尝试手动唤起")
}
```

这里摘抄核心代码，对于`android`新建`iframe`元素指向跳转链接，其他的比如苹果直接丢到地址栏。如果没有跳转，后面的代码执行就会提示文字信息。

除了这段代码，原始代码里还需要检查各种环境，比如是在微信里打开，在`vivo`手机等？如果在`点评APP`里打开调用 `launchApplication`等。这里介绍android原始的浏览器处理方式

```html
<iframe src="imeituan://www.meituan.com/web?lch=wind_Nzk5NmRhMWQt___sms_134157_1597819375908728873___t_wind_b6a9ac6de011&amp;t_wind=t_wind_b6a9ac6de011&amp;url=https%3A%2F%2Fcube.meituan.com%2Fcube%2Fblock%2F37256092d86f%2F194126%2Findex.html%3Flch%3Dwind_Nzk5NmRhMWQt___sms_134157_1597819375908728873___t_wind_b6a9ac6de011%26src%3DDX%26t_wind%3Dt_wind_b6a9ac6de011" style="display: none;"></iframe>
```

跳转地址是这里的src, `imeituan`应该是美团应用注册在手机里的协议地址。

```html
imeituan://www.meituan.com/web?lch=wind_Nzk5NmRhMWQt___sms_134157_1597819375908728873___t_wind_b6a9ac6de011&t_wind=t_wind_b6a9ac6de011&url=https%3A%2F%2Fcube.meituan.com%2Fcube%2Fblock%2F37256092d86f%2F194126%2Findex.html%3Flch%3Dwind_Nzk5NmRhMWQt___sms_134157_1597819375908728873___t_wind_b6a9ac6de011%26src%3DDX%26t_wind%3Dt_wind_b6a9ac6de011

```

这个地址除了用户的定位信息（lch和t_wind）外，跳转页面`url`，做一下`URLDecode`

```
https://cube.meituan.com/cube/block/37256092d86f/194126/index.html?lch=wind_Nzk5NmRhMWQt___sms_134157_1597819375908728873___t_wind_b6a9ac6de011&src=DX&t_wind=t_wind_b6a9ac6de011
```

用浏览器打开这个页面，展示活动页面后很快跳转到美团登陆界面，显然需要鉴权。

至此如何唤起`app`的原理大抵弄清，通过手机浏览器的地址跳转功能，加上`app`注册的协议地址和`app`关联。传递跳转的内部页面地址，当然也可以是其他的参数。

美团`app`里大部分页面是`h5`页面，这样可以减少开发成本，除了可以用于`app`，还可以放到小程序里，放到浏览器里浏览，微信中传播。
