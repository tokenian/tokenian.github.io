---
title: "论数学对编程的影响（四）"
subtitle: "再论代码"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/post-bg-infinity.jpg"
tags:
  - life
  - math
  - philosophy
---

```c++
int a = 1;
bool b = true;
int c = 2;
bool d = a > c;

if (b && d) {
    世界毁灭
} else {
    上帝降临
}
```

上面的代码里，使用了[布尔代数](https://zh.wikipedia.org/wiki/%E5%B8%83%E5%B0%94%E4%BB%A3%E6%95%B0)。

几乎现在通行的编程语言都有布尔逻辑运算，与运算（`&&`）,或运算（`||`），非运算（`!`）。还有排中律，也就是非黑即白，比如前面代码里变量d，它可能为true，也可能为false，只能是二者之一。理解布尔代数最形象的莫过于以前学的电学里的串流和并流，串流只要其中一个开关没打开，就无法通电；而并流是只要打开一个就能通电。虽然爱情只能是一个恒态，正如仓央嘉措说的，`你爱或者不爱我，爱就在那里，不增不减。`

```
a = [1, 2, 3, 4, 5, 6]
b = [2, 4, 6, 8, 10]
c = a + b = [1, 2, 3, 4, 5, 6, 8, 10]
d = a - b = [1, 3, 5]
e = a ^ b = [2, 4, 6]
```

上面的代码演示了基本的集合操作， 交集、差集、并集。

要想作为一名合格的程序员，对集合的正确使用是必不可少的。编程的世界里定义了一些常用的数据结构，`List`列表，`Set`集合，`Map`键值对，`Stack`栈，`Queue`队列。虽然有点眼花缭乱，不过说到底，它们骨子里还是一堆同类数据凑到一块。栈和队列内部依然是集合，只是表现的行为有些差异而已。

在代码的汪洋大海里，数学的化身俯拾即是。数学给了代码光与火，让程序员有了光明和温暖。

曾经有一位先哲，他认为数学的本质是逻辑推理。数学定理的证明，往往由一堆堆公式运算加上逻辑推理而得到。为此，他参与编写了几本牛皮大书。他就是罗素，[逻辑主义](https://zh.wikipedia.org/wiki/%E9%82%8F%E8%BC%AF%E4%B8%BB%E7%BE%A9)的代表人物。罗素最广为人知的是他的理发师悖论，`理发师为村里不能给自己理发的人理发`。悖论的问题就是，理发师到底该不该给他自己理发呢？罗素参与了数学中的逻辑主义建设。

> 在[数学](https://zh.wikipedia.org/wiki/數學)里，逻辑是指[形式逻辑](https://zh.wikipedia.org/wiki/逻辑#概念)和[数理逻辑](https://zh.wikipedia.org/wiki/数理逻辑)，形式逻辑是研究某个[形式语言](https://zh.wikipedia.org/wiki/形式語言)的有效[推论](https://zh.wikipedia.org/wiki/推论)[[3\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-stanford-logic-onthology-3)。主要是演绎推理。 在[辩证法](https://zh.wikipedia.org/wiki/辯證法)中也涉及到逻辑[[4\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-4)。数理逻辑是研究抽象逻辑关系和数学基本的问题。
>
> **形式逻辑** 是对命题、陈述或断然使用的句子和演绎论证的抽象研究[[17\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-Formallogic-19)。 是研究纯形式内容的[推论](https://zh.wikipedia.org/wiki/推论)的一门学科，这种内容是很明确的。若一个推论可以被表达成一个完全抽象的规则（即不只是和任一特定事物或性质有关的规则）的一个特定应用，则这个推论拥有**纯形式内容**。形式逻辑的规则由[亚里斯多德](https://zh.wikipedia.org/wiki/亞里斯多德)最先写成[[18\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-20)。在许多逻辑的定义中，逻辑推论与带有纯形式内容的推论会是同一种概念。但这不表示非形式逻辑的概念是空洞的，因为没有任何一种形式语言可以捕捉到自然语言语义间所有的微细差别。 [形式](https://zh.wikipedia.org/wiki/形式_(哲學))是逻辑的核心，但在“形式逻辑”中对“形式”使用时常不很明确，因而使其阐述变得很费解。其中，符号逻辑仅为形式逻辑的一种类型，而和形式逻辑的另一种类型－只处理[直言命题](https://zh.wikipedia.org/wiki/直言命題)的[三段论](https://zh.wikipedia.org/wiki/三段論)不同。[[19\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-21) 
>
> **[数理逻辑](https://zh.wikipedia.org/wiki/數理邏輯)**是符号逻辑在其他领域中的延伸，特别是对[模型论](https://zh.wikipedia.org/wiki/模型論)、[证明论](https://zh.wikipedia.org/wiki/證明論)、[集合论](https://zh.wikipedia.org/wiki/集合論)和[递归论](https://zh.wikipedia.org/wiki/遞歸論)的研究。
>
> 摘自维基百科。

程序员写代码，代码之间是蕴含逻辑推断的。比如现在很多商家使用的付款二维码，不管你是用微信、支付宝或者云闪付扫码，它都能自动识别，唤起对应的支付页面，这都是通过传递过来的地址协议分析出来的。共享单车的二维码里包含那辆车唯一的ID,扫码的同时会通过ID查询该车是否损坏，是否参加红包活动，当前用户是否月卡、周卡用户。电视盒子的遥控板，天猫精灵的语音指令。。。

逻辑的幽灵，

在指间飘荡，

在黑色的眸子中跳跃，

是光与火，

照亮了晶体管前行的路！

