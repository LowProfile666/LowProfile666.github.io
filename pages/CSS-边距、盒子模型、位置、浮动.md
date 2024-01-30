# 9、内边距与外边距

先定义一个 div，并给他设置样式，观察页面：

```html
<div class="div-outer">
    <div class="div-inner"></div>
</div>
<style>
    .div-outer {
        width: 300px;
        height: 300px;
        background-color: lightblue;
    }
</style>
```

观察页面会发现，div 展示出来的矩形并没有紧靠着窗口的边缘，而是留有一定的空隙。使用开发者工具，用箭头选择点击这个 div，在控制台的 body 区域就能看到一个盒子样式的模型，就可以看到 div 外面的外边距、边框、内边距、内容：

![image-20240130101936553](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301025924.png)

## **`margin`**

`margin` 属性为给定元素设置所有四个（上下左右）方向的外边距属性。

+ 可以接受1~4个值（上、右、下、左的顺序），多个参数的赋值规则同边框`border`。

+ 可以分别指明四个方向：`margin-top`、`margin-right`、`margin-bottom`、`margin-left`

+ 可取值

  + `length`：固定值

  + `percentage`：相对于包含块的宽度，以百分比值为外边距。

  + `auto`：让浏览器自己选择一个合适的外边距。有时，在一些特殊情况下，该值可以使元素居中。

    水平居中：只设置上下边距，左右边距使用 `auto`，这种方法只对块级元素有效。

    ```css
    margin: 0 auto;
    ```

+ 外边距重叠

  + 块的上外边距(`margin-top`)和下外边距(`margin-bottom`)有时合并(折叠)为单个边距，其大小为单个边距的最大值(或如果它们相

    等，则仅为其中一个)，这种行为称为边距折叠。

  + 父元素与后代元素：父元素没有上边框和`padding`时，后代元素的`margin-top`会溢出，溢出后父元素的`margin-top`会与后代元素取最大值。

去除外边距就是将外边距赋值为 0px。

```html
<div class="div-outer" style="margin:0px;">
    <div class="div-inner"></div>
</div>
```

但是有时候浏览器可能会有一些默认的外边距样式，比如 `body` 区域会有个 8px 的默认外边距，那么以上代码就可能不起作用，所以一般会这样写来清除所有默认的外边距样式：

```css
* {
    margin: 0px;
}
```

### 外边距重叠1：

当我们给里面的div设置一个外边距后：

```css
* {
    margin: 0px;
}
.div-outer {
    width: 300px;
    height: 300px;
    background-color: lightblue;
}
.div-inner {
    width: 100px;
    height: 100px;
    background-color: darkred;
    margin: 20px;
}
```

会发现页面上出现这种效果：

![image-20240130104258345](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637170.png)此时就是上外边距发生了重叠。红色的div的外边距影响到了蓝色div的外边距，导致蓝色div也多了一个外边距。这是因为==蓝色div作为父元素没有上边框和`padding`时，后代元素的`margin-top`会溢出，溢出后父元素的`margin-top`会与后代元素取最大值。== 所以只要给蓝色div设置一个上边框就可以解决，注意，上边框不能设为 0px：

```css
.div-outer {
    border-top: 1px solid;
}
```

![image-20240130104742471](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637074.png)

但是不管是设置父级元素的边框或者内边距时，都会影响到原来的元素高度，会让元素高度增加，所以还可以使用 `overflow` 来抵消这种外边距重叠的效果。

可以把子元素溢出的外边距当成是子元素的一部分，超出了父元素的顶部范围，所以就使用 `overflow:hidden;` 将它溢出部分给隐藏了，这样就可以了。

但是万一我们这个父元素需要将超出的部分显示出来，这时候就不能使用 `overflow:hidden` 了，还有一个方法，就是在父元素的前面再加一个元素即可。

```css
.div-outer::before {
    content: "";
    display: table;
}
```

注意，直接在HTML文档中添加一个元素是不行的：

```html
<div style="display: table;"></div>
<div class="div-outer">
    <div class="div-inner"></div>
</div>
```

### 外边距重叠2：

有两个元素 A、B，当 A 元素有一个对于 B 的下外边距时，且 B 元素有一个对于 A 的上外边距时，这时他们的外边距会发生重叠，会取最大值。

比如，再定义一个 div：

```html
<div class="div-outer">
    <div class="div-inner"></div>
    <div class="div-inner2"></div>
</div>
```

设置样式：

```css
.div-inner {
    width: 100px;
    height: 100px;
    background-color: darkred;
    margin-bottom: 20px;
}
.div-inner2 {
    width: 100px;
    height: 100px;
    background-color: darkcyan;
    margin-top: 30px;
}
```

此时页面上两个 div 的间距就是 30px，因为它取了最大值。

所以说外边距其实就是在某一个方向上，与其他元素相隔的最小距离。

另外，重叠只会出现在上下外边距上，左右外边距是不会有的。

```css
.div-inner {
    width: 100px;
    height: 100px;
    background-color: darkred;
    float: left;
    margin-right: 20PX;
}
.div-inner2 {
    width: 100px;
    height: 100px;
    background-color: darkcyan;
    float: left;
    margin-left: 20PX;
}
```

此时 A、B 元素之间的间距是 40px。

## **`padding`**

内边距是定义元素中的内容离某个方向上元素的边界的最小距离。

注意：**内边距会影响到元素的大小，元素大小（宽高）会加上内边距。**

+ 默认的，一个矩形的宽度为：内边距的宽度 + 内容的宽度 + 边框的宽度

`padding` CSS 简写属性控制元素所有四条边的内边距区域。

+ 可以接受1~4个值（上、右、下、左的顺序）
+ 可以分别指明四个方向：`padding-top`、`padding-right`、`padding-bottom`、`padding-left`
+ 可取值
  + `length`：固定值
  + `percentage`：相对于包含块的宽度，以百分比值为内边距。

# 10、盒子模型

## **`box-sizing`**

元素的总宽度 = 内边距的宽度 + 内容的宽度 + 边框的宽度

CSS 中的 `box-sizing` 属性定义了 user agent 应该如何计算一个元素的总宽度和总高度。

+ `content-box`：是默认值，设置`border`和`padding`均会增加元素的宽高。
+ `border-box`：设置`border`和`padding`不会改变元素的宽高，而是挤占内容区域。

# 11、位置

## **`position`**

CSS `position`属性用于指定一个元素在文档中的定位方式。

**流式渲染：**就是页面上的元素一个挨着一个的被渲染，是 `static` 的效果。

**定位类型：**

+ 定位元素（positioned element）是其计算后位置属性为 `relative`, `absolute`, `fixed `或 `sticky `的一个元素（换句话说，除`static`以外的任何东西）。
+ 相对定位元素（relatively positioned element）是计算后位置属性为 `relative` 的元素。
+ 绝对定位元素（absolutely positioned element）是计算后位置属性为 `absolute` 或 `fixed` 的元素。
+ 粘性定位元素（stickily positioned element）是计算后位置属性为 `sticky` 的元素。

**取值：**

+ `static`：该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 `top`, `right`, `bottom`, `left `和 `z-index` 属性无效。
  + `z-index`：是指向屏幕外的坐标轴，可以用来控制元素的层次显示
+ `relative`：该关键字下，元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。`top`, `right`, `bottom`, `left`等调整元素相对于初始位置的偏移量。
+ `absolute`：元素会被移出正常文档流，并不为元素预留空间，通过指定元素相对于最近的非 `static` 定位祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（`margins`），且不会与其他边距合并。
+ `fixed`：元素会被移出正常文档流，并不为元素预留空间，而是通过指定元素相对于屏幕视口（`viewport`）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变。
+ `sticky`：元素根据正常文档流进行定位，然后相对它的最近滚动祖先（nearest scrolling ancestor）和 containing block (最近块级祖先 nearest block-level ancestor)，包括`table-related`元素，基于`top`, `right`, `bottom`, 和 `left`的值进行偏移。偏移值不会影响任何其他元素的位置。

元素：

```html
<div class="div-outer">
    <div class="div-inner1">1</div>
    <div class="div-inner2">2</div>
    <div class="div-inner3">3</div>
</div>
```

## **static**

流式渲染，一个挨着一个渲染，就是默认的情况

```html
.div-inner1 {
    width: 50px;
    height: 50px;
    background-color: darkred;
    color: white;
    margin: 10px;
    display: inline-block;
}

.div-inner2 {
    width: 50px;
    height: 50px;
    background-color: darkred;
    color: white;
    display: inline-block;
    margin: 10px;
}

.div-inner3 {
    width: 50px;
    height: 50px;
    background-color: darkred;
    color: white;
    display: inline-block;
    margin: 10px;
}
```

## **relative**

元素有一个最初始所在的位置，然后在 `realative` 下，使用 `top`、`right`、`bottom`、`left` 可以控制元素上下左右地相对于原来地位置进行一个偏移，偏移的距离是以初始的位置为起点计算的，所以“相对”是相对于元素的初始位置。

```html
.div-inner2 {
    width: 50px;
    height: 50px;
    background-color: darkgreen;
    color: white;
    display: inline-block;
    margin: 10px;
    position: relative;
    top: 100px;
    right: 40px;
}
```

并且，该元素原来的位置不会被其他元素占据，依旧保持在那里。

![image-20240130113217725](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637770.png)

使用 `z-index` 实现层次感：

```css
.div-inner1 {
    width: 50px;
    height: 50px;
    background-color: darkred;
    color: white;
    margin: 10px;
    display: inline-block;
}

.div-inner2 {
    width: 50px;
    height: 50px;
    background-color: darkgreen;
    color: white;
    display: inline-block;
    margin: 10px;
    position: relative;
    right: 60px;
}

.div-inner3 {
    width: 50px;
    height: 50px;
    background-color: darkblue;
    color: white;
    display: inline-block;
    margin: 10px;
    position: relative;
    right: 120px;
}
```

此时效果，此时的 1 被 2、3 盖住了：

![image-20240130114212243](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301142318.png)

使用 `z-index` 将 div1 盖住 div2、div3：

```css
position: relative;
z-index: 1;
```

此时因为默认的 `z-index` 都是 0，所以给 div1 的 `z-index` 设为 1 就可以盖住其他两个div，如果 div2 的 `z-index` 是 1 的话，那么 div1 的 `z-index` 就要设为 2 才能盖住 div2。

## **absolute**

`absolute` 是对应整个文档绝对定位，就是说，元素会随着文档的滚动而滚动。

`absolute` 定位时是从父节点开始向上找，会找到第一个非 `static` 的祖宗节点来定位。

在最开始的样子，我们给 div2 添加绝对定位：

```css
.div-inner2 {
    width: 50px;
    height: 50px;
    background-color: darkgreen;
    color: white;
    display: inline-block;
    position: absolute;
    top: 0px;
    left: 0px;
}
```

效果：

![image-20240130120045073](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301200149.png)

发现 div2 在指定 `top` 和 `left` 偏移为 0 的时候，是对齐窗口的左上顶点的，说明当前 `absolute` 是相对文档来定位的。因为它的父级 div 默认是 `static`.

如果我们把 div2 的父级元素的定位设为非 `static`，同时为了方便看效果，将最外层的 div 设置一个上外边距，此时效果：

![image-20240130115848261](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637617.png)

这就能说明，`absolute` 是相对第一个非 `static` 祖宗节点来定位的。同时，也能发现， div2 这个元素已经被文档流中踢出来了，div3 会占掉 div2 在流中应该占据的位置。

## **fixed**

固定定位，也是从父节点开始向上找，找到第一个非 `static` 的元素来定位。

固定定位的元素不会随着文档的滚动而滚动。

```css
.div-inner2 {
    width: 50px;
    height: 50px;
    background-color: darkgreen;
    color: white;
    display: inline-block;
    position: fixed;
    top: 200px;
    right: 0px;
}
```

可以实现这样的效果：

![image-20240130120604316](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637775.png)

## **sticky**

粘性定位，效果类似Excel中的冻结窗口。

```html
<div class="div-outer">
    <div class="div-inner2">2</div>
    <div class="div-inner1">1</div>
    <div class="div-inner2">2</div>
    <div class="div-inner3">3</div>
    <div class="div-inner2">2</div>
</div>
```

设置 div1 为粘性定位

```css
.div-inner1 {
    width: 50px;
    height: 50px;
    background-color: darkred;
    color: white;
    margin: 10px;
    position: sticky;
    top: 20px;
}
```

以上样式能实现的效果：当页面向下滚动时，当 div1 区域滚动到距离窗口顶部还剩 20px 距离时，就会停止滚动，div1 就会一直处于当前距离顶部 20px 的位置，但是页面的其他元素不会停止滚动。

设置 div3 为粘性定位

```css
.div-inner3 {
    width: 50px;
    height: 50px;
    background-color: darkblue;
    color: white;
    margin: 10px;
    position: sticky;
    bottom: 20px;
}
```

以上样式能实现的效果：当页面向上滚动时，当 div3 区域滚动到距离窗口底部还剩 20px 距离时，就会停止滚动，div3 就会一直处于当前距离底部 20px 的位置，但是页面的其他元素不会停止滚动。

# 12、浮动

现在有很多 div 块：

```html
<div class="div-outer">
    <div class="div-inner2">2</div>
    <div class="div-inner1">1</div>
    <div class="div-inner2">2</div>
    <div class="div-inner3">3</div>
    <div class="div-inner2">2</div>
    <div class="div-inner3">3</div>
    <div class="div-inner3">3</div>
    <div class="div-inner3">3</div>
    <div class="div-inner3">3</div>
    <div class="div-inner3">3</div>
    <div class="div-inner3">3</div>
    <div class="div-inner3">3</div>
</div>
```

要想这些 div 块实现一种效果就是当一行能放下这些块的时候九放在一行，放不下的话就换行放，可以实现 `inline-block` 实现：

```css
.div-inner1 {
    width: 50px;
    height: 50px;
    background-color: darkred;
    color: white;
    display: inline-block;
}

.div-inner2 { 
    width: 50px;
    height: 50px;
    background-color: darkgreen;
    color: white;
    display: inline-block;
}

.div-inner3 {
    width: 50px;
    height: 50px;
    background-color: darkblue;
    color: white;
    display: inline-block;
}
```

效果：

![image-20240130131434998](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637822.png)

可以看到，我们并没有设置外边距属性，但是使用 `inline-block` 属性的话，一行中自带了一个空隙，如果想不要这个空隙，就要使用浮动来实现。将 `display:inline-block` 换成以下代码：

```css
float: left;
```

效果：

![image-20240130131723204](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637009.png)

## **`float`**

`float` CSS属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动(文档流)中移除，尽管仍然保持部分的流动性（与绝对定位相反）。

由于float意味着使用块布局，它在某些情况下修改 `display` 值的计算值：

+ `display`为`inline`或`inline-block`时，使用`float`后会统一变成`block`。

**取值：**

+ `left`：表明元素必须浮动在其所在的块容器左侧的关键字。
+ `right`：表明元素必须浮动在其所在的块容器右侧的关键字。

使用浮动后，该元素在页面上的位置就会被其他元素占据，使用浮动就相当于这个元素浮在了原来位置的上方，所以其他元素是可以占据它下方的原来的位置。

在上面代码的基础中，在添加一个新的div块,，设置样式：

```css
.div-inner4 {
    width: 100px;
    height: 100px;
    background-color: yellow;
}
```

然后看页面效果：

![image-20240130132419700](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637019.png)

并没有看到我们的黄色div块，这是因为div4被放在了左上角四个格子的下面，因为这四个格子是浮动的，所以他们下方的位置可以被占据，如果把浏览器窗口拉大一点，就能看见下面的div4：

![image-20240130132603864](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637939.png)

或者使用 `z-index` 也可以看见div4：

```css
.div-inner4 {
    width: 100px;
    height: 100px;
    background-color: yellow;
    position: relative;
    z-index: 1;
}
```

![image-20240130132702369](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301637984.png)

如果我们要将新的元素放在浮动元素的后面而不是下面的话，就需要使用 `clear`。

## **`clear`**

有时，你可能想要强制元素移至任何浮动元素下方。比如说，你可能希望某个段落与浮动元素保持相邻的位置，但又希望这个段落从头开始强制独占一行。此时可以使用`clear`。

**取值：**

+ `left`：清除左侧浮动。
+ `right`：清除右侧浮动。
+ `both`：清除左右两侧浮动

```css
.div-inner4 {
    width: 100px;
    height: 100px;
    background-color: yellow;
    clear: left;
}
```

![image-20240130132819222](https://gitee.com/LowProfile666/image-bed/raw/master/img/202401301328305.png)