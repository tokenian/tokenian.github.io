---
title: "学习开源仓库make-real"
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

[make-real](https://github.com/tldraw/make-real)工具提供一种可能，把用户的设计意图用前端代码来呈现。具体用法也很简单，用户在白板上画草图，使用各种几何形状，配上文字说明，点击右侧的Make Real按钮，页面上就会出现预览框。预览框里的图片是通过调用OPENAI的gpt-4-turbo模型，把用户画的草图，文字，发送给gpt-4-Vision模型，生成用tailwind css框架为主的html代码，代码经由tldraw转换为canvas图片。

#### [tldraw](https://github.com/tldraw/tldraw)

这是一个挺厉害的react canvas库，白板呈现功能依赖此库。***persistenceKey*** 指定浏览器本地存储，支持同步。shapeUtils支持自定义形状。***ExportButton*** 即为make real按钮。[帮助文档](https://tldraw.dev/quick-start)

```
		<div className="tldraw__editor">
			<Tldraw
				persistenceKey="tldraw"
				shapeUtils={shapeUtils}
				components={{ SharePanel: () => <ExportButton /> }}
			>
				<APIKeyInput />
				<LinkArea />
			</Tldraw>
		</div>
```

AI模型生成的html代码更新到canvas

```
		// Update the shape with the new props
		editor.updateShape<PreviewShape>({
			id: newShapeId,
			type: 'preview',
			props: {
				html,
				source: dataUrl as string,
				linkUploadVersion: 1,
				uploadedShapeId: newShapeId,
			},
		})
```

#### 提示词工程

ChatGPT的chat接口支持多种角色消息，包括***'system' | 'user' | 'assistant' | 'function'*** 。下面摘抄项目里用到的提示词，具有学习价值.

```
You are an expert web developer who specializes in building working website prototypes from low-fidelity wireframes. Your job is to accept low-fidelity designs and turn them into interactive and responsive working prototypes. When sent new designs, you should reply with a high fidelity working prototype as a single HTML file.

Use tailwind CSS for styling. If you must use other CSS, place it in a style tag.

Put any JavaScript in a script tag. Use unpkg or skypack to import any required JavaScript dependencies. Use Google fonts to pull in any open source fonts you require. If you have any images, load them from Unsplash or use solid colored rectangles as placeholders. 

The designs may include flow charts, diagrams, labels, arrows, sticky notes, screenshots of other applications, or even previous designs. Treat all of these as references for your prototype. Use your best judgement to determine what is an annotation and what should be included in the final result. Treat anything in the color red as an annotation rather than part of the design. Do NOT include any of those annotations in your final result.

Your prototype should look and feel much more complete and advanced than the wireframes provided. Flesh it out, make it real! Try your best to figure out what the designer wants and make it happen. If there are any questions or underspecified features, use what you know about applications, user experience, and website design patterns to "fill in the blanks". If you're unsure of how the designs should work, take a guess—it's better for you to get it wrong than to leave things incomplete. 

Remember: you love your designers and want them to be happy. The more complete and impressive your prototype, the happier they will be. Good luck, you've got this!
```

这是system角色的提示词，详细告诉AI系统如何处理用户的图像文字输入，如果处理不了怎么办，Javascript和CSS，chats、diagrams、labels元素。

```
	"Here are the latest wireframes. There are also some previous outputs here. We have run their code through an 'HTML to screenshot' library, that attempts to generate a screenshot of the page. The generated screenshot may have some inaccuracies, so use your knowledge of HTML and web development to figure out what any annotations are referring to, which may be different to what is visible in the generated screenshot. Make a new website based on these wireframes and notes and send back just the HTML file contents."
```

```
Here are the latest wireframes. There are also some previous outputs here. Could you make a new website based on these wireframes and notes and send back just the html file?
```

这是默认的用户提示词。下面的是根据用户的输入来判断提示词。用户的输入可以迭代式的，不断的修改，发送给AI系统的数据可以包含前面的内容，使其更好地理解用户意图。

```
if (text) {
		userContent.push({
			type: 'text',
			text: `Here's a list of all the text that we found in the design. Use it as a reference if anything is hard to read in the screenshot(s):\n${text}`,
		})
	}

```

如果用户输入了文字

```
if (grid) {
		userContent.push({
			type: 'text',
			text: `The designs have a ${grid.color} grid overlaid on top. Each cell of the grid is ${grid.size}x${grid.size}px.`,
		})
	}

```

如果用户使用了网格

```
	// Prompt the theme
	userContent.push({
		type: 'text',
		text: `Please make your result use the ${theme} theme.`,
	})
```

用户选择了主题，比如现在流行的暗黑和明亮模式

```
	// Add the previous previews as HTML
	for (let i = 0; i < previousPreviews.length; i++) {
		const preview = previousPreviews[i]
		userContent.push(
			{
				type: 'text',
				text: `The designs also included one of your previous result. Here's the image that you used as its source:`,
			},
			{
				type: 'image_url',
				image_url: {
					url: preview.props.source,
					detail: 'high',
				},
			},
			{
				type: 'text',
				text: `And here's the HTML you came up with for it: ${preview.props.html}`,
			}
		)
	}
```

前面的修改内容

```
	const body: GPT4VCompletionRequest = {
		model: 'gpt-4-turbo',
		max_tokens: 4096,
		temperature: 0,
		messages,
		seed: 42,
		n: 1,
	}
	
	try {
		const resp = await fetch('https://api.openai.com/v1/chat/completions', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				Authorization: `Bearer ${apiKey}`,
			},
			body: JSON.stringify(body),
		})
		json = await resp.json()
	} catch (e) {
		throw Error(`Could not contact OpenAI: ${e.message}`)
	}
```

GPT请求内容，这里的seed和n参数我不理解。

#### 总结

GPT4的接口调用简单，难的是打造产品细节的能力。这个仓库曾经很火，它的出现，宣示了基于UI草图生成前端代码的可能性。而事实上，目前有很多不错的工具支持从设计稿来写前端。

程序员被AI替代只是时间上的问题
