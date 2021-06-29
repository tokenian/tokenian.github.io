---
title: "论数学对计算机的深远影响（三）"
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

上面的代码里，使用了布尔代数。

![image-20210629221454896](https://gitee.com/tokenian/images-bed/raw/master/img/image-20210629221454896.png)

几乎现在通行的面向对象编程语言都有布尔逻辑运算，与运算（`&&`）,或运算（`||`），非运算（`!`）。还有排中律，也就是非黑即白，比如前面代码里变量d，它可能为true，也可能为false，只能是二者之一。

编程语言里存在大量的集合操作，`List、Set、Map`,集合之间可能产生交集、并集、差集等。掌握集合类库的使用是每一个程序员的必修课。

毫无疑问，它又是数学里的东西。数学给了编程语言光与火，让程序员有了光明和温暖。

曾经有一位先驱，他认为数学的本质是逻辑推理。数学定理的证明，往往由一堆堆公式运算加上逻辑推理而得到。为此，他还参与写了几本厚厚的书。他就是罗素，逻辑主义的代表人物。罗素最广为人知的是他的理发师悖论，理发师给不能给自己理发的人理发，理发师到底该不该给自己理发呢？罗素参与了数学中的逻辑主义建设。

![image-20210629222940211](https://gitee.com/tokenian/images-bed/raw/master/img/image-20210629222940211.png)

> 在[数学](https://zh.wikipedia.org/wiki/數學)里，逻辑是指[形式逻辑](https://zh.wikipedia.org/wiki/逻辑#概念)和[数理逻辑](https://zh.wikipedia.org/wiki/数理逻辑)，形式逻辑是研究某个[形式语言](https://zh.wikipedia.org/wiki/形式語言)的有效[推论](https://zh.wikipedia.org/wiki/推论)[[3\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-stanford-logic-onthology-3)。主要是演绎推理。 在[辩证法](https://zh.wikipedia.org/wiki/辯證法)中也涉及到逻辑[[4\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-4)。数理逻辑是研究抽象逻辑关系和数学基本的问题。
>
> **形式逻辑** 是对命题、陈述或断然使用的句子和演绎论证的抽象研究[[17\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-Formallogic-19)。 是研究纯形式内容的[推论](https://zh.wikipedia.org/wiki/推论)的一门学科，这种内容是很明确的。若一个推论可以被表达成一个完全抽象的规则（即不只是和任一特定事物或性质有关的规则）的一个特定应用，则这个推论拥有**纯形式内容**。形式逻辑的规则由[亚里斯多德](https://zh.wikipedia.org/wiki/亞里斯多德)最先写成[[18\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-20)。在许多逻辑的定义中，逻辑推论与带有纯形式内容的推论会是同一种概念。但这不表示非形式逻辑的概念是空洞的，因为没有任何一种形式语言可以捕捉到自然语言语义间所有的微细差别。 [形式](https://zh.wikipedia.org/wiki/形式_(哲學))是逻辑的核心，但在“形式逻辑”中对“形式”使用时常不很明确，因而使其阐述变得很费解。其中，符号逻辑仅为形式逻辑的一种类型，而和形式逻辑的另一种类型－只处理[直言命题](https://zh.wikipedia.org/wiki/直言命題)的[三段论](https://zh.wikipedia.org/wiki/三段論)不同。[[19\]](https://zh.wikipedia.org/wiki/逻辑#cite_note-21) 
>
> **[数理逻辑](https://zh.wikipedia.org/wiki/數理邏輯)**是符号逻辑在其他领域中的延伸，特别是对[模型论](https://zh.wikipedia.org/wiki/模型論)、[证明论](https://zh.wikipedia.org/wiki/證明論)、[集合论](https://zh.wikipedia.org/wiki/集合論)和[递归论](https://zh.wikipedia.org/wiki/遞歸論)的研究。
>
> 摘自维基百科。

编程语言里到处游荡着逻辑的幽灵，布尔判断，集合运算。

