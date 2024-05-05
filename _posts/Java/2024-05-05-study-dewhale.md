---
title: "学习开源仓库dewhale"
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

[**dewhale**](https://github.com/Yuyz0112/dewhale),这是另一个**AI**代码生成工具，基于**Github**的工作流来完成用户需求迭代。使用流程是用户在**Github**仓库上提*issue*，可以用文字、图片描述需求，打上特定的标签。**Github**工作流监听此种事件，获取用户的输入，依据打上的标签附带上开发库的使用样例，调用**OPENAI**的生成接口，生成代码，后期利用**AST**检查代码是否存在语法错误，修正，在同一个*issue*下追加*pull request*，附带评论。

相比**make-real**的提示词，这个工具的提示词更加庞大，比如为了使用组件库***@shadcn/ui***，提示词里包含了几乎所有的组件使用样例。

#### [Settings](https://probot.github.io/apps/settings/)

这是**Github**上的app，支持用户在仓库里利用.github/settings.yml作个性化设置。

```
labels:
  - name: ui-gen
    color: '#61dbfb'
    description: will trigger a React UI generating workflow

  - name: vue-ui-gen
    color: '#41b883'
    description: will trigger a Vue UI generating workflow

  - name: svelte-ui-gen
    color: '#aa1e1e'
    description: will trigger a Svelte UI generating workflow

  - name: bug
    color: '#d73a4a'
    description: Something isn't working

  - name: documentation
    color: '#0075ca'
    description: Improvements or additions to documentation
```

定义了不同的*label*，其中***ui-gen*** (react)  ***vue-ui-gen*** (vue) ***svelte-ui-gen*** (svelte) 用来生成不同的前端代码。用户提交或者修改issue时可以选择需要的*label*

#### workflow

**Github**的工作流通常用来帮助用户作自动化构建(**DevOps**)

```
on:
  issues:
    types: [edited, labeled, unlabeled]
  issue_comment:
    types: [created, edited, deleted]
  pull_request_review_comment:
    types: [created, edited, deleted]
jobs:
  handle_issue:
    runs-on: ubuntu-latest
    name: Create PR
    if: ${{ !contains(github.event.comment.body, '[vx.dev]') && !contains(github.event.comment.body, '[Dewhale]') && github.event.comment.user.type != 'bot'}}
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      - name: Setup Deno
        uses: denoland/setup-deno@v1
        with:
          deno-version: 1.38.3
      - name: generate UI
        env:
          # auto set by GitHub, details in
          # https://docs.github.com/en/actions/security-guides/automatic-token-authentication
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          WHITELIST: ${{ secrets.WHITELIST }}
          CONFIG: ${{ vars.CONFIG }}
          ACTOR: ${{ github.actor }}
        run: deno run -A prompts/entry.ts
```

这里监听几种类型的事件，触发```deno run -A prompts/entry.ts```。这个项目使用了*deno*, *nodejs*的又一个版本。entry.ts根据用户打的标签生成不同的模板。下面以ui-gen为例。

#### ui-gen

系统提示词放在***vue-ui-gen.md***文件中，包含了功能描述，组件库使用样例。

````
You are an expert web developer who specializes in building working website prototypes. Your job is to accept low-fidelity wireframes and instructions, then turn them into interactive and responsive working prototypes. When sent new designs, you should reply with your best attempt at a high fidelity working prototype as a SINGLE static Vue file, which export a default component as the UI implementation.

### Available Component 1, accordion:

```jsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
<Accordion type="single" collapsible>
  <AccordionItem value="item-1">
    <AccordionTrigger>Is it accessible?</AccordionTrigger>
    <AccordionContent>
      Yes. It adheres to the WAI-ARIA design pattern.
    </AccordionContent>
  </AccordionItem>
</Accordion>;
```
````

系统提示词如此庞杂的原因，是提供尽可能多的信息，增加生成内容的确定性。

```
const { code, usage, description } = await getCode(
    [
      {
        role: "system",
        content: systemPrompt,
      },
      {
        role: "user",
        content: [
          {
            type: "text",
            text: prompt,
          },
          ...images.map(
            (image) =>
              ({
                type: "image_url",
                image_url: {
                  url: image,
                },
              } as const)
          ),
        ],
      },
    ],
    "gpt-4-vision-preview"
  );
```

准备提示词，包含系统提示词，用户输入的文本、图片。使用*gpt-4-vision-preview*模型，生成代码**code**,描述**description**，使用**usage**

```
await applyPR(
    owner,
    repo,
    issue.number,
    branch,
    {
      "vue-preview-ui/src/Preview.vue": refineCode(code),
      "scripts/build-task": "vue-preview-ui",
    },
    `${dewhalePrefix} prompt:\r\n${commitMsg}`,
    [vueUiGenLabel]
  );
```
提交PR。其中的**refindCode**函数即是**AST**语法树精炼代码。
```
 if (description) {
    await octokit.rest.issues.createComment({
      owner,
      repo,
      issue_number: pr.number,
      body: `${dewhalePrefix}: ${description}`,
    });
  }
```

提交评论。评论中包含特定关键字**dewhalePrefix** ([Dewhale])，用于工作流过滤。

#### 总结

这个项目发起的初衷是模仿**v0.dev**，*Vercel*开发的一个代码生成网站。背后的原理是**RAG**(Retrieval Augmented Generation)，通过检索获取相关的知识并将其融入Prompt，让大模型能够参考相应的知识从而给出合理回答。**RAG**的优势是无需微调模型，嵌入使用者的知识库，达到较好的泛化结果。

作为普通的开发者，没有成本去训练大模型，没有精力去研究神经网络结构，只能做一些应用上的创新。同前面的**make-real**项目一样，**Dewhale**在代码生成的层面更进一步。从这个项目可以看出，开发者在**RAG**领域深耕也是大有可为。
