# 关于CSS中Flex布局的技术讨论

Flexible Box 模型，通常被称为 flexbox，是一种一维的布局模型。它给 flexbox 的子元素之间提供了强大的空间分布和对齐能力。

## flexbox的两个轴线

当使用flex布局的时候，我们首先要想到两根轴线。

主轴和交叉轴。

主轴由`flex-direction`定义。交叉轴垂直于它。

### 主轴

主轴有四种属性：

* row
* row-reverse
* column
* column-reverse

如果使用row或者row-reverse属性，主轴将沿着inline（内联）方向延伸。

![If flex-direction is set to row the main axis runs along the row in the inline direction.](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox/basics1.png)

如果选择column或者column-reverse属性，主轴将会沿着上下方向延伸，也就是block（块）排列的方向。

![If flex-direction is set to column the main axis runs in the block direction.](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox/basics2.png)

### 交叉轴

交叉轴垂直于主轴，如果使用row或者row-reverse属性，交叉轴就是沿着列向下。

![If flex-direction is set to row then the cross axis runs in the block direction.](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox/basics3.png)

如果选择column或者column-reverse属性，交叉轴就是水平方向。

![If flex-direction is set to column then the cross axis runs in the inline direction.](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox/basics4.png)

flexbox 的特性是沿着主轴或者交叉轴对齐之中的元素。

### 起始线和终止线

不同语言的书写方式不同，有的从左到右，有的从右到左，flexbox不对书写方式有要求，所以一般可以用起始（start）和终止（end）来描述我们常识中的左右。

## Flex容器

为了创建flex容器，我们可以使用display属性并设置值为flex或者inline-flex。

设置完后，容器内的直系子元素就会变成flex元素。所有 CSS 属性都会有一个初始值，所以 flex 容器中的所有 flex 元素都会有下列行为：

- 元素排列为一行 (`flex-direction` 属性的初始值是 `row`)。
- 元素从主轴的起始线开始。
- 元素不会在主维度方向拉伸，但是可以缩小。
- 元素被拉伸来填充交叉轴大小。
- flex-basis属性为auto。
- flex-wrap属性为nowrap。

这会让你的元素呈线形排列，并且把自己的大小作为主轴上的大小。如果有太多元素超出容器，它们会溢出而不会换行。如果一些元素比其他元素高，那么元素会沿交叉轴被拉伸来填满它的大小。

### 更改flex方向属性flex-direction

在 flex 容器中添加flex-direction属性可以让我们更改 flex 元素的排列方向。设置 `flex-direction: row-reverse` 可以让元素沿着行的方向显示，但是起始线和终止线位置会交换。

把 flex 容器的属性 `flex-direction` 改为 `column` ，主轴和交叉轴交换，元素沿着列的方向排列显示。改为 `column-reverse` ，起始线和终止线交换。

### 用flex-wrap实现多行flex容器

虽然`flexbox`是一维模型，但可以使我们的`flex`项目应用到多行中。在这样做的时候，您应该把每一行看作一个新的`flex`容器。任何空间分布都将在该行上发生，而不影响该空间分布的其他行。

flex-wrap属性设置为wrap的时候，如果容器内的子元素的宽度大于容器本身的最大宽度，那么子元素将换行显示。

如果设置成nowrap的话，子元素如果允许缩小（设置了初始Flexbox值），则会优先缩小以适应容器，否则会`溢出`。

### 简写属性flex-flow

可以将flex-direction和flex-wrap这两个属性组合在一起作为简写属性flex-flow。

第一个指定的值是flex-direction，第二个指定的值是flex-wrap。

### flex元素上的属性



