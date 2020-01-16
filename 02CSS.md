### CSS 基础

#### CSS 选择器的优先级

css 选择器的优先级是：内联 > ID 选择器 > 类选择器 > 标签选择器

具体的计算，优先级是由 A、B、C、D 的值来决定的，其中它们的值计算规则如下:

- 当存在内联样式时 A 等于 1，否则 A=0
- B 的值等于 ID 选择器出现的次数
- C 的值等于类选择器和属性选择器和伪类选择器出现的总次数
- D 的值等于标签选择器和伪元素选择器出现的总次数

就比如下面的选择器，它不存在内联样式，所以 A 等于 0，不存在 id 选择器 B=0，存在一个类选择器 C=1，存在三个标签选择器 D=3，那么最终的计算结果为：{0,0,1,3}

```
ul ol li .red {
  ...
}
```

按照这个结算方式，下面的计算结果为:{0,1,0,0}

```
#red {
  ...
}
```

我们的比较优先级的方式是从 A 到 D 去比较值的大小，A，B，C，D 权重从左到右，依次减少。判断优先级时从左到右，一一比较，知道比较出最大值，即可停止

比如第二个例子的 B 与第一个例子相比，1>0，接下来就不需要比较了，第二个选择器的优先级高

#### link 和@import 的区别？

- link 属于 XHTML 标签，而@import 是 CSS 提供的
- 页面被加载时，link 会同时被加载，而@import 引用的 CSS 会等到页面被加载完再加载
- import 只在 IE5 以上才能识别，而 link 是 XHTML 标签，无兼容问题
- link 方式的样式权重高于@import 的权重
- 使用 dom 控制样式时的差别。当使用 JS 控制 dom 去改变样式的时候，只能使用 link 标签，因为@import 不是 dom 可以控制的

#### 有哪些方式（CSS）可以隐藏页面元素

- `opactity:0` : 本质上将元素的透明度设置为 0，看起来是隐藏了，但是依然占据空间且可以交互
- `visibility:hidden` : 与上一个方法类似的效果，占据空间，但是不可以交互了
- `overflow:hidden` : 这个只隐藏溢出的部分，但是占据空间且不可交互
- `display:none` : 这个是彻底隐藏了元素，元素从文档流中消失，既不占据空间也不能交互，也不影响布局
- `z-index: -9999` : 原理是将层级放到底部，这样就被覆盖了，看起来隐藏了
- `transform: scale(0,0)` : 平面变换，将元素缩放为 0，但是依然占据空间，但不可交互

#### em\px\rem 区别

- px: 绝对单位，页面按精确像素展示
- em: 相对单位，基准点为父节点字体的大小，如果自身定义了 font-size 按自身来计算，整个页面内 1em 不是一个固定的值
- rem: 相对单位，可理解为"root em",相对根节点 html 的字体大小来计算，CSS3 新加属性

#### 水平垂直居中的方法

- 方法 1：绝对定位+margin:auto

```
div {
  width: 200px;
  height: 200px;
  background: red;

  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
```

- 方法 2：绝对定位+负 margin

```
div{
  width: 200px;
  height: 200px;
  background: red;

  position: absolue;
  left: 50%;
  top: 50%;
  margin-left: -100px;
  margin-top: -100px;
}

```

- 方法 3：绝对定位+transform

```
div{
  width: 200px;
  height: 200px;
  background: red;

  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
```

- 方法 4：flex 布局

```
.box{
  height: 600px;
  width: 600px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.box > div{
  width: 200px;
  height: 200px;
  background: red;
}
```

- 方法 5：table-cell

```
.box {
  display: table;
  height: 600px;
  width: 600px;
}
.box > div {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
```

#### css 有几种定位方式

- static: 正常文档流定位，此时 top,right,bottom,left 和 z-index 属性无效，块级元素从上往下纵向排序，行内元素从左到右排序
- relative: 相对定位，此时的相对是相对于正常的文档流的位置
- absolute: 相对于最近的非 static 定位祖先元素的偏移，来确定位置，比如一个绝对定位的元素它的父级，和祖先父级都为 releative，它会相对于父级而产生偏移
- fixed: 指定元素相对于屏幕视口（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变，比如那种回到顶部的按钮一般都是用此定位方式
- sticky: 粘性定位，特性近似于 relative 和 fixed 的合体

#### 如何理解 z-index

css 的 z-index 控制重叠元素的垂直叠加顺序，我们可以修改元素的 z-index 来控制元素的图层位置，而且 z-index 只能影响设置了 position 的元素（除了 static）

#### 如何理解层叠上下文

层叠上下文是 HTML 元素的三维概念，这些 HTML 元素在一条假想的相对于面向（电脑屏幕的）视窗或者网页的用户的 z 轴上延伸，HTML 元素依据其自身属性按照优先级顺序占用层叠上下文的空间

#### 清除浮动的方式

- 空 div 的方式，可以在浮动元素后面添加一个空 div，并设置 clear:both
- 伪元素方式，在父元素上添加 after 伪元素，并设置 clear: both
- 给父元素创建 BFC

#### 你对 css sprites 的理解，好处是什么

雪碧图也叫精灵图，是 css 图像合成技术，开发人员往往将图标合到一张图片上，这个图片称作雪碧图

好处: 可以减少多张图片的 http 请求，可以提前加载资源

不足: 当其中一个图标需要改变时需要改动这张图片，加载速度优势在 http2 开启后荡然无存，HTTP2 多路复用，多张图片也可以重复利用一个连接通道搞定

#### 你对媒体查询的理解

媒体查询由一个可选的媒体类型和零个或多个使用媒体功能的限制了样式表范围的表达式组成，例如宽度、高度和颜色。媒体查询，添加自 CSS3，允许内容的呈现针对一个特定范围的输出设备而进行裁剪，而不必改变内容本身,非常适合 web 网页应对不同型号的设备而做出对应的响应适配。

#### 你对盒模型的理解

当对一个文档进行布局的时候，浏览器的渲染引擎会根据标准之一的 CSS 基础框盒模型，将所有元素表示为一个个矩形的盒子。CSS 决定这些盒子的大小、位置以及属性（例如颜色、背景、边框尺寸…）

盒模型由 content（内容）、padding（内边距）、border（边框）、margin（外边距）组成

#### 标准盒模型和 IE 盒模型有什么区别

可以通过 box-sizing 来定义盒子的模型

- 标准盒模型中(content-box)，我们定义的 width 就是元素的 content 的宽度，height 就是元素 content 的高度，即盒子的宽度 = width + padding + border
- IE 盒模型(border-box)，我们定义的 width 就是 content + padding + border，height 同理

#### 谈谈对 BFC 的理解

- 定义: BFC 是块级格式化上下文，它是页面中的一块渲染区域，有一套渲染规则，决定了其子元素如何布局，以及和其他元素之间的关系和作用。
- 作用: BFC 是页面上的一个隔离的容器，里面的元素不会影响到外面元素的布局，反之亦然。1.可以阻止元素被浮动元素覆盖。2.可以包含浮动元素。3.解决边距重叠
- 触发条件: 1.float 值不为 none。2.overflow 不为 visiable。3.diplay 的值为 inline-block,table-cell,table-caption,flex,inline-flex。4.position 的值为 absolue 或 fixed

#### 为什么使用 translate 来改变位置而不是定位

translate()是 transform 的一个值。改变 transform 或 opacity 不会触发浏览器重新布局（reflow）或重绘（repaint），只会触发复合（compositions）。而改变绝对定位会触发重新布局，进而触发重绘和复合。transform 使浏览器为元素创建一个 GPU 图层，但改变绝对定位会使用到 CPU。 因此 translate()更高效，可以缩短平滑动画的绘制时间

而 translate 改变位置时，元素依然会占据其原始空间，绝对定位就不会发生这种情况

#### 伪元素和伪类的区别

伪类（pseudo-class） 是一个以冒号(:)作为前缀，被添加到一个选择器末尾的关键字，当你希望样式在特定状态下才被呈现到指定的元素时，你可以往元素的选择器后面加上对应的伪类。

伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如说，我们可以通过::before 来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。

区别：伪类是通过在元素选择器上加入伪类改变元素状态，而伪元素通过对元素的操作进行对元素的改变
