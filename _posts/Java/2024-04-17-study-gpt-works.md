---
title: "学习开源仓库gpt-works"
subtitle: "看下别人的技术栈"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/home-bg-f.jpg"
tags:
  - web
  - extension
  - llma
---

> 通过学习开源仓库，了解当下的技术栈，潮流趋势。文章只是作为个人的学习笔记，所以不具备很好的可读性。旨在将来哪天个人查阅

#### GPTs Works（https://github.com/all-in-aigc/gpts-works.git）

去年ChatGPT所在的公司开放了GPT插件市场，允许用户自由创建应用，利用其提供的AI能力变现。据说几天的时间就有百万应用冒出来，那么问题就来了，如何找到你想要的GPT插件呢？微信公众号推荐了一篇文章，作者是个创业者，他用几天的时间开发出了一个谷歌插件、Web网站，帮助使用者找到感兴趣的插件。

代码放在github上，于是clone下来看看到底怎么回事。

项目分三部分，web、index、extension。中间的index使用的是向量数据库来为GPT插件创建索引，另外两个看名字即是web网站和谷歌插件。

我感兴趣的是作者如何几天内完成这个项目

#### web

技术栈用的是react+nextjs+tailwindcss+typescript。

nextjs比较好上手，按目录来设定路由，类似文件夹一样。

typescript里的async和await关键字很好用，避免了回调地狱和promise的嵌套，简洁。

nextjs有许多优秀的前沿特性，比如静态编译、动态编译，和Vercel搭配良好。学习上手快。

/app/api/ 下定义各种请求API ,一个简单的GET请求如下定义

```
export async function GET()
```

界面用tailwindcss控制，比较美观。我的windows跑不起来，使用的依赖包不支持。

页面上请求index索引服务来获取GPT插件

#### index

这块用python写的，FastAPI+llama_index。GPT插件用对象来描述，有各种属性，对其中关键的描述description建立索引。使用的正是llama_index,存储于云服务上。

这个项目很简单，关键点是llama_index这个工具。对插件文本描述建立向量数据库。

//每个GPT插件一个文档

```
document = Document(
        text=gpts.description,
        metadata={
            "uuid": gpts.uuid,
            "name": gpts.name,
            "author_name": gpts.author_name,
        },
        metadata_template="{key}: {value}",
        text_template="{metadata_str}\ndescription: {content}",
    )
```
//构建索引

```
 storage_context = get_storage_context()
 
    index = VectorStoreIndex(nodes=nodes, storage_context=storage_context)

    print("save index ok")
```
//index 检索
```
  storage_context = get_storage_context()
    index = VectorStoreIndex.from_documents([], storage_context=storage_context)

    retriever = VectorIndexRetriever(index=index, similarity_top_k=10)

    nodes = retriever.retrieve(question)
```



这个向量数据库有一个使用场景是QA，对用户的提问做召回检索。检索出Top n，之后对n条数据和用户提问向量再次做关联word embeding。

#### extension

技术栈react+plasmo+tailwindcss。

值得一提的就是浏览器插件快速开发框架plasmo。它提供了很多简便的工具，写后台background

```
const handler: PlasmoMessaging.MessageHandler = async (req,res)=> {}
```

发送消息给后台

```
sendToBackground
```

content页面监听消息

```
  useMessage<string, string>(async (req, res) //监听
  sendToContentScript  //发送消息给content
```

原理依然是请求index提供的索引服务。

#### 总结

把自己认为值得借鉴的记录了下。该作者使用了一些开源的快开发框架，使用了前沿的向量数据库。react的状态管理用的是原生的useState，如果用mobx开发会更快。
