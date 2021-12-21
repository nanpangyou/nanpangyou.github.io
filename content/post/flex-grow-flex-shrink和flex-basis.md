---
title: "flex-grow,flex-shrink和flex-basis"
categories:
  - Note
  - daily
tags:
  - note
date: 2020-01-06 11:24:37
---

flex 有三个属性值，分别是 flex-grow， flex-shrink， flex-basis，默认值是 0 1 auto

# flex-basis

和 width 一样，他的默认值为 auto，如果和 width 属性同时设置一个值，flex-basis 会比较优先。当然工作中最好用 flex-basis，更符合规范。

# flex-grow

该属性来设置，当父元素的宽度大于所有子元素的宽度的和时（即父元素会有剩余空间），子元素如何分配父元素的剩余空间。
flex-grow 的默认值为 0，意思是该元素不索取父元素的剩余空间，如果值大于 0，表示索取。值越大，索取的越厉害。

例如:
父元素宽 400px，有两子元素：A 和 B。A 宽为 100px，B 宽为 200px。

则空余空间为 400-（100+200）= 100px。

如果 A，B 都不索取剩余空间，则有 100px 的空余空间。

如果 A 索取剩余空间:设置 flex-grow 为 1，B 不索取。则最终 A 的大小为 自身宽度（100px）+ 剩余空间的宽度（100px）= 200px

如果 A，B 都设索取剩余空间，A 设置 flex-grow 为 1，B 设置 flex-grow 为 2。
则最终 A 的大小为 自身宽度（100px）+ A 获得的剩余空间的宽度（100px \* (1/(1+2))）
最终 B 的大小为 自身宽度（200px）+ B 获得的剩余空间的宽度（100px \* (2/(1+2))）

计算方式就是一共分成所有的 grow 只和份，每一个占用几分之几

# flex-shrink

该属性来设置，当父元素的宽度小于所有子元素的宽度的和时（即子元素会超出父元素），子元素如何缩小自己的宽度的。

flex-shrink 的默认值为 1，当父元素的宽度小于所有子元素的宽度的和时，子元素的宽度会减小。值越大，减小的越厉害。如果值为 0，表示不减小。

例如:
父元素宽 400px，有两子元素：A 和 B。A 宽为 200px，B 宽为 300px。

则 A，B 总共超出父元素的宽度为(200+300)- 400 = 100px。

如果 A，B 都不减小宽度，即都设置 flex-shrink 为 0，则会有 100px 的宽度超出父元素。

如果 A 不减小宽度:设置 flex-shrink 为 0，B 减小。则最终 B 的大小为 自身宽度(300px)- 总共超出父元素的宽度(100px)= 200px

如果 A，B 都减小宽度，A 设置 flex-shirk 为 3，B 设置 flex-shirk 为 2。
最终 A 的大小为 自身宽度(200px)- A 减小的宽度(100px \* (200px \* 3/(200 \* 3 + 300 \* 2))) = 150px
最终 B 的大小为 自身宽度(300px)- B 减小的宽度(100px \* (300px \* 2/(200 \* 3 + 300 \* 2))) = 250px

# 注意

如果父级的空间足够：flex-grow 有效，flex-shrink 无效。

如果父级的空间不够：flex-shrink 有效，flex-grow 无效。

# 参考：

[flex-grow、flex-shrink、flex-basis 详解](https://www.jianshu.com/p/0642dfe0e571)
[深入理解 flex-grow & flex-shrink & flex-basis](https://segmentfault.com/a/1190000006741711)
