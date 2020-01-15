---
title: "计算机-数学的延伸"
subtitle: "哥德尔不完备定理揭示了虚无"
layout: post
author: "tokenian"
# header-style: text
header-img: "img/home-bg-f.jpg"
tags:
  - math
  - philosophy
  - software
---

> 计算机，这里讨论的不仅仅是人们常说的电脑，更是泛指它的衍生物。比如软件、硬件、量子计算机、机器人等。本文是[数学：确定性的丧失](https://book.douban.com/subject/1049136//)这本书的读后感及思考

[哥德尔不完备定理]([https://zh.wikipedia.org/wiki/%E5%93%A5%E5%BE%B7%E5%B0%94%E4%B8%8D%E5%AE%8C%E5%A4%87%E5%AE%9A%E7%90%86](https://zh.wikipedia.org/wiki/哥德尔不完备定理))开启了数学的潘多拉魔盒，它告诉世人：数学的相容性和完备性不可兼得。

数学的三次危机先说下

第一次是毕达哥拉斯学派的唯数论，万物皆数，数是一切的尺度。很直白的说，数是从真实世界抽象出来的。但是学派里的一个人搞出了根号2，1.41421。 这个数也被称作白银分割，有白银自然有黄金分割。毕达哥拉斯学派证明了这个数既不是偶数，也不是奇数，这违背了学派的信仰。发现这个数的人被投海了，因为他发现了魔鬼。

第二次是莱布尼茨的无穷小量，一个比任意正实数小的量，同时在必要的时候可以丢弃。很明显这是矛盾的，我们知道实数的连续性。假设这个量为a，a >0。比a小的数必然存在，同时为何能随意丢弃。对于追求准确性的的数学来说是不合逻辑的。学过高数的我们知道，数学家用极限来解决了无穷小。但是微积分建立在直觉上成立的假设上，这堆假设没有经过证明。19世界数学家对数学做了大量的公理化建设，从柯西开始。把数学的各个分支的基础都夯实了。数学家觉得终于把数学弄确定了，不再根基不稳了

第三次是康托的集合论公理。著名的罗素理发师悖论，一个理发师只给村里不给自己理发的人理发，那么他应不应该给自己理发呢？不给自己理发的人是一个集合，但是理发师他自己属不属于这个集合呢？由此诞生了数学哲学里的几大学派。罗素的逻辑主义，希尔伯特的形式主义，布劳威尔的直觉主义，策梅罗的公理化集合理论。

但是哥德尔的不完备性定理击碎了数学家的期待，你们的证明是不可能的。一个相容性的形式系统T，假如含有数论的公理，则必然存在一个属于该系统的S断言，S可能为真，S可能为假，但是从T系统里的公理无法证明它，它是不可证的。

软件的编程语言也是一个形式系统，一堆人为定义的符号，有一些规则，它们就是这套系统的公理。但是目前来了个问题，至于能不能用该语言去解决是无法证明，无法预知的。这或许也是软件漏洞永远补不完的原因，总有矛盾产生。当你解决了一个矛盾，又一个新的矛盾被你无意中创造，而你全然不知。

断言：在一个形式系统内的一个论断，要么为真，要么为假。比如算术系统内1+1=2，为真。

相容性：一个系统内的公理彼此之间独立，不会通过一些公理推导出其他公理是错的。非欧几何因为第五公理的不可判定性而产生，内部是不产生矛盾的。同欧氏几何一样是自洽的、相容的。

完备性：涉及一个系统的断言，必然可以用该系统的公理通过有限步去推导证明。哥德尔不完备定理说，你证明不了。

不可判定性：一个断言是否可以通过一个判定程序经过有限步的推导来判定，它是可以被证明的。就好比计算机的NP问题，至今无解。它肯定不可判定，但是否可解可证明呢？

数学是科学的皇后，化学、物理学、经济学等学科都是在数学的基础上建立的。如果说数学的系统是不完备的，而数学作为它们的基石，那就是说它们也是不完备的。也就是说学法律的人总有办法歪曲法律，因为总有条文、事实是法律解释不了、处理不了的。这也是老实人吵架始终干不过巧舌如簧的人，秀才遇到兵有理说不清。

计算机，它的设计理念来源于数学。冯.诺伊曼，现代计算机架构的设计者，希尔伯特的学生，很自然的运用了形式主义的哲学。在我们的编程语言中，除了形式语言的符号，还有逻辑主义的逻辑。布尔逻辑，蕴含，推断，迭代，统统是数学里的东西。

数学是心智产生的东西，最开始来源于经验，它不是绝对的真理。数学是无限的接近真理，只能这样说。老子说，大成若缺。人的心智始终存有缺憾，无法找到上帝的设计准则。正如一首歌，翻唱和改调可能更好听。这就是一个残缺的世界！