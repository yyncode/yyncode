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

## 起始线和终止线

不同语言的书写方式不同，有的从左到右，有的从右到左，flexbox不对书写方式有要求，所以一般可以用起始（start）和终止（end）来描述我们常识中的左右。

