###问答
####1.浮动元素有什么特征？对父容器、其他浮动元素、普通元素、文字分别有什么影响?

浮动的框可以左右移动（根据float属性值而定），直到它的外边缘碰到包含框或者另一个浮动元素的框的边缘。浮动元素不在文档的普通流中，文档的普通流中的元素表现的就像浮动元素不存在一样.

- 对父容器的影响:父容器的子元素都是浮动元素，并且父容器没有定义高度，那么父容器会失去高度，在浮动元素之外。
- 对其他浮动元素:其他浮动元素能感知到浮动元素.如果父级容器太窄,无法包容所有浮动元素,那么其他浮动元素会向向下移动,知道有足够空间.
- 对于普通元素，普通元素会感知不到浮动元素存在，如果宽高合适，后面的元素会占据浮动元素原来的位置.
- 对文字的影响：文字会感知到浮动元素的存在，会留出空间，形成环绕效果。

####2.清除浮动指什么? 如何清除浮动? 两种以上方法

清除浮动是指让父元素在视觉上包围浮动元素。
清除浮动的方法:
1. 如果我们想让父元素在视觉上包围浮动元素,可以在代码最后添加一个`<div style="clear:both;"></div>`对它清理。缺点是增加了一个无意义的标签.
2. BFC清理浮动:因为BFC可以包含浮动，因此可以让父元素生成一个新的BFC从而包围浮动的子元素。可以对父元素设定以下样式之一生成新BFC

```
float: left | right;
overflow: hidden | auto | scroll;
display: table-cell | table-caption | inline-block;
position: absolute | fixed;
```

####3.有几种定位方式，分别是如何实现定位的，参考点是什么，使用场景是什么？


- **inherit:** 规定应该从父元素继承 position 属性的值

- **static:**  默认值,没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明

**relative**: 生成相对定位的元素，相对于元素本身正常位置进行定位,因此，left:20px 会向元素的 left - 位置添加20px

- **absolute:** 生成绝对定位的元素，相对于static定位以外的第一个祖先元素（offset parent）进行定位,元素的位置通过 left, top, right 以及bottom属性进行规定.

- **fixed:** 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 left, top, right 及 bottom 属性进行规定

- **sticky:** CSS3新属性，表现类似position:relative和position:fixed的合体，在目标区域在屏幕中可见时，它的行为就像position:relative; 而当页面滚动超出目标区域时，它的表现就像position:fixed，它会固定在目标位置



>注意:sticky兼容性特别不好,不推荐使用.效果可以用javascript来实现.


####4.z-index 有什么作用? 如何使用?
因为绝对定位的元素脱离了普通流，所以绝对定位的元素可以覆盖页面上的其它元素。这时可以通过给元素设置z-index属性来控制叠放顺序，该属性值越高，元素位置越靠上。

####position:relative和负margin都可以使元素位置发生偏移?二者有什么区别
position:relative;只相对自己原本位置发生偏移，不影响其它普通流中元素的位置。
负margin：除了让元素自身发生偏移还影响其它普通流中的元素。


####5.BFC 是什么？如何生成 BFC？BFC 有什么作用？举例说明
- BFC是块级格式上下文.它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。BFC布局规则：
>- 内部的Box会在垂直方向，一个接一个地放置。
>- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
>- 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
>- BFC的区域不会与float box重叠。
>- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
>- 计算BFC的高度时，浮动元素也参与计算

- 哪些元素会生成BFC?
>- 根元素
>- float属性不为none
>- position为absolute或fixed
>- display为inline-block, table-cell, table-caption, flex, inline-flex
>- overflow不为visible

- BFC 有什么作用？举例说明
    1. 自适应两栏布局.如下列代码:
```
<style>
    body {
        width: 300px;
        position: relative;
    }
 
    .aside {
        width: 100px;
        height: 150px;
        float: left;
        background: #f66;
    }
 
    .main {
        height: 200px;
        background: #fcc;
    }
</style>
<body>
    <div class="aside"></div>
    <div class="main"></div>
</body>
```
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4585153-3f614b0f8f0fa99c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由于存在浮动的元素aslide，main感知不到浮动元素的存在,仍然与包含快左边抵触.
但添加`.main {overflow: hidden}`后,
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4585153-3a65eadbc836d40c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
触发形成BFC,main不会与浮动的aside重叠.

    2. 清除内部浮动:对父容器添加触发BFC的属性来形成BFC达到“清浮动”效果
```
<div style="border: solid 5px #0e0; width:300px;overflow:hidden;">
  <div style="height: 100px; width: 100px; background-color: Red;  float:left;">
  </div>
  <div style="height: 100px; width: 100px; background-color: Green;  float:left;">
  </div>
  <div style="height: 100px; width: 100px; background-color: Yellow;  float:left;">
  </div>
</div>
```

![4585153-d43a0208ec04b88b.png](http://upload-images.jianshu.io/upload_images/4585153-f352e15f0b473e3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####6在什么场景下会出现外边距合并？如何合并？如何不让相邻元素外边距合并？给个父子外边距合并的范例
块级元素的 上外边距（[margin-top
](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top)） 与 下外边距（[margin-bottom
](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom)） 有时会合并（塌陷）为单个外边距，这样的现象称之为 **外边距合并(塌陷)。**如下三种情况会形成外边距合并:
1. 毗邻兄弟元素
毗邻的两个兄弟元素之间的外边距会塌陷（当后者被 [清除浮动](https://developer.mozilla.org/en-US/docs/CSS/clear) 时除外）
2. 块级父元素与其第一个/最后一个子元素
如果块级父元素中，不存在上边框、上内补
、inline content、[清除浮动
](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)这四条属性（对于上边框和上内补（padding-top)，也可以说，当上边距及上内补宽度为0时），那么这个块级元素和其第一个子元素的上边距就可以说”挨到了一起“。此时这个块级父元素和其第一个子元素就会发生 上外边距合并 现象，换句话说，此时这个父元素对外展现出来的外边距将直接变成这个父元素和其第一个子元素的margin-top的较大者。
3. 空块元素
如果存在一个空的块级元素，其 [border
](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border)、[padding
](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding)、inline content、[height
](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height)、[min-height
](https://developer.mozilla.org/zh-CN/docs/Web/CSS/min-height) 都不存在。那么此时它的上下边距中间将没有任何阻隔，此时它的上下外边距将会合并。

以下为父子外边距合并的案例:
```<html>
<head>

<style type="text/css">
* {
  margin:0;
  padding:0;
  border:0;
}

#outer {
  width:300px;
  height:300px;
  background-color:red;
  margin-top:20px;

}

#inner {
  width:50px;
  height:50px;
  background-color:blue;
  margin-top:80px;
}

</style>
</head>

<body>

<div id="outer">
  <div id="inner">
  </div>
</div>
```
效果为:

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4585153-be40b411dca7344c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看到,即使外容器设置了`margin-top:20px;`,而内容器设置了`margin-top:80px;`由于外容器不存在上边框、上内补、inline content、清除浮动这四条属性,发生了外边距合并,外容器离html顶端的距离取值为二者margin-top 较大的那个,为80px,而不是20px.

---
###代码
1. [alert效果](https://lycii1.github.io/web_development/taskwork/task10-1.html)
2. [表单效果](https://lycii1.github.io/web_development/taskwork/task10-2.html)
3. [模拟框效果](https://lycii1.github.io/web_development/taskwork/task10-3.html)
4. [导航栏效果](https://lycii1.github.io/web_development/taskwork/task10-4.html)
