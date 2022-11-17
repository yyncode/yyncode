# 关于CSS中Flex布局的技术讨论

Flexible Box 模型，通常被称为 flexbox，是一种一维的布局模型。它给 flexbox 的子元素之间提供了强大的空间分布和对齐能力。

## flexbox的两个轴线

当使用flex布局的时候，我们首先要想到两根轴线。

主轴和交叉轴。

主轴由`flex-direction`定义。交叉轴垂直于它。

### 主轴（flex-direction）

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

####  **可用空间** available space

假设在 1 个 500px 的容器中，我们有 3 个 100px 宽的元素，那么这 3 个元素需要占 300px 的宽，剩下 200px 的可用空间。在默认情况下，flexbox 的行为会把这 200px 的空间留在最后一个元素的后面。

![This flex container has available space after laying out the items.](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox/basics7.png)

如果期望这些元素能自动地扩展去填充满剩下的空间，那么我们需要去控制可用空间在这几个元素间如何分配，这就是元素上的那些 `flex` 属性要做的事。

#### Flex元素：flex-basis

flex-basis定义了该元素的**空间大小**（**the size of that item in terms of the space**），flex 容器里除了元素所占的空间以外的富余空间就是**可用空间** available space。

默认值是 `auto` 

如果没有给元素设定尺寸，`flex-basis` 的值采用元素内容的尺寸。这就解释了：我们给只要给 Flex 元素的父元素声明 `display: flex` ，所有子元素就会排成一行，且自动分配小大以充分展示元素的内容。

#### Flex元素属性：flex-grow

`flex-grow` 若被赋值为一个正整数，flex 元素会以 `flex-basis` 为基础，沿主轴方向增长尺寸。这会使该元素延展，并占据此方向轴上的可用空间（available space）。如果有其他元素也被允许延展，那么他们会各自占据可用空间的一部分。

flex-grow 属性可以按比例分配空间。如果第一个元素 `flex-grow` 值为 2，其他元素值为 1，则第一个元素将占有 2/4（上例中，即为 200px 中的 100px）, 另外两个元素各占有 1/4（各 50px）。

#### Flex元素属性：flex-shrink

`flex-grow`属性是处理 flex 元素在主轴上增加空间的问题，相反`flex-shrink`属性是处理 flex 元素收缩的问题。如果我们的容器中没有足够排列 flex 元素的空间，那么可以把 flex 元素`flex-shrink`属性设置为正整数来缩小它所占空间到`flex-basis`以下。与`flex-grow`属性一样，可以赋予不同的值来控制 flex 元素收缩的程度 —— 给`flex-shrink`属性赋予更大的数值可以比赋予小数值的同级元素收缩程度更大。

在计算 flex 元素收缩的大小时，它的最小尺寸也会被考虑进去，就是说实际上 flex-shrink 属性可能会和 flex-grow 属性表现的不一致。

#### Flex属性的简写

`Flex` 简写形式允许你把三个数值按这个顺序书写 — `flex-grow`，`flex-shrink`，`flex-basis`。

第一个数值是 `flex-grow`。赋值为正数的话是让元素增加所占空间。第二个数值是`flex-shrink` — 正数可以让它缩小所占空间，但是只有在 flex 元素总和超出主轴才会生效。最后一个数值是 `flex-basis`；flex 元素是在这个基准值的基础上缩放的。

下面是几种预定义的值：

- `flex: initial`
- `flex: auto`
- `flex: none`
- `flex: <positive-number>`

`flex: initial` 是把 flex 元素重置为 Flexbox 的初始值，它相当于 `flex: 0 1 auto`。在这里 `flex-grow` 的值为 0，所以 flex 元素不会超过它们 `flex-basis` 的尺寸。`flex-shrink` 的值为 1, 所以可以缩小 flex 元素来防止它们溢出。`flex-basis` 的值为 `auto`. Flex 元素尺寸可以是在主维度上设置的，也可以是根据内容自动得到的。

`flex: auto` 等同于 `flex: 1 1 auto`；和上面的 `flex:initial` 基本相同，但是这种情况下，flex 元素在需要的时候既可以拉伸也可以收缩。

`flex: none` 可以把 flex 元素设置为不可伸缩。它和设置为 `flex: 0 0 auto` 是一样的。元素既不能拉伸或者收缩，但是元素会按具有 `flex-basis: auto` 属性的 flexbox 进行布局。

### 元素间的对齐和空间分配

Flexbox 的一个关键特性是能够设置 flex 元素沿主轴方向和交叉轴方向的对齐方式，以及它们之间的空间分配。

#### align-items

align-items属性可以使元素在交叉轴方向对齐。

这个属性的初始值为`stretch`，这就是为什么 flex 元素会默认被拉伸到最高元素的高度。实际上，它们被拉伸来填满 flex 容器 —— 最高的元素定义了容器的高度。

可以设置`align-items`的值为`flex-start`，使 flex 元素按 flex 容器的顶部对齐，`flex-end` 使它们按 flex 容器的下部对齐，或者`center`使它们居中对齐。

- `stretch`
- `flex-start`
- `flex-end`
- `center`

#### justify-content

justify-content属性用来使元素在主轴方向上对齐，主轴方向是通过flex-direction设置的方向。

初始值是`flex-start`，元素从容器的起始线排列。但是你也可以把值设置为`flex-end`，从终止线开始排列，或者`center`，在中间排列。

你也可以把值设置为`space-between`，把元素排列好之后的剩余空间拿出来，平均分配到元素之间，所以元素之间间隔相等。或者使用`space-around`，使每个元素的左右空间相等。

- `stretch`
- `flex-start`
- `flex-end`
- `center`
- `space-around`
- `space-between`

这些只是基础部分，详情见[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)