---
layout: learnnote
title: css属性
date: 2019-11-13 18:12:29
---

# 遇见的，不会的，CSS 属性

1. backface-visibility

   功能： 隐藏被旋转的 div 元素的背面

   ```
   <style>
   div
   {
   position:relative;
   height:60px;
   width:60px;
   border:1px solid #000;
   background-color:yellow;
   transform:rotateY(180deg);
   -webkit-transform:rotateY(180deg); /* Chrome and Safari */
   -moz-transform:rotateY(180deg); /* Firefox */
   }
   #div1
   {
   -webkit-backface-visibility:hidden;
   -moz-backface-visibility:hidden;
   -ms-backface-visibility:hidden;
   }
   #div2
   {
   -webkit-backface-visibility:visible;
   -moz-backface-visibility:visible;
   -ms-backface-visibility:visible;
   }
   </style>
   </head>
   <body>

   <p>本例有两个 div 元素，均旋转 180 度，背向用户。</p>

   <p>第一个 div 元素的 backface-visibility 属性设置为 "hidden"，所以应该是不可见的。</p>

   <div id="div1">DIV 1</div>

   <div id="div2">DIV 2</div>

   ```

   [can i use](https://www.caniuse.com/#search=backface-visibility)

2. user-select
   
   功能：可以使元素中的文字不被选中,在日常需求中,以一些需求会要求比如链接等地方的文字不可被选中，这个时候就可以使用这个属性

   ```
   <style>
      a{
         user-select: none;
      }
   </style>
   </head>
   <body>

   <a href='#'>链接链接链接链接链接</a>

   ```
   该属性有5个值： 
   - none(元素和子元素的文本将无法被选中)  
   - all(当所有内容作为一个整体时可以被选择。如果双击或者在上下文上点击子元素，那么被选择的部分将是以该子元素向上回溯的最高祖先元素)  
   - auto(默认)  
   - text(文本可以被选中)    
   - contain、element(可以选择文本，但选择范围受元素边界的约束，也就是选择的文本将包含在该元素的范围内。只支持Internet Explorer)

   [can i use](https://www.caniuse.com/#search=user-select)

3. pointer-events
   
   功能：This CSS property, when set to "none" allows elements to not receive hover/click events, instead the event will occur on anything behind it.

   ```
   <style>
   div{
      pointer-events: none;
   }
   div:hover{
      color: orange;
   }
   </style>
   </head>
   <body>

   <div>
      链接：<a href='http://www.bilibili.com'>bilibili</a>
   </div>

   ```
   注意点：

   - pointer-events 的值为 none 时，如果元素上绝对定位，那在它下一层的元素可以被选中。
   - pointer-events: none; 只是用来禁用鼠标的事件，通过其他方式绑定的事件还是会触发的，比如键盘事件等。
   - 如果将一个元素的子元素 pointer-events 设置成其他值（比如：auto），那么当点击子元素时，还是会通过事件冒泡的形式出发父元素的事件。
   
   [can i use](https://www.caniuse.com/#search=pointer-events)