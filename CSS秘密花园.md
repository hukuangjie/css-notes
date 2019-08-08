#  斑马条纹背景的文本行

《[CSS Secrets](http://shop.oreilly.com/product/0636920031123.do)》是[@Lea Verou](http://leaverou.com/)最新著作，这本书讲解了有关于CSS中一些小秘密。

几年前，我们第一次接触伪元素`:nth-child()`/`:nth-of-type()`的时候，最常见的用例之一就是“斑马条纹”的表格。

这在以前需要服务器端的代码，客户端的脚本，或繁琐的手工代码，现在它已经只需要简单的这几行代码就可以实现了：

```css
tr:nth-child(even) {
    background: rgba(0,0,0,.2);
}
```

但是，对于为文本行应用相同的效果，而不是表格中的一行的时候，我们就变得无能为力了。这对于提高代码片段的可读性非常有用。很多作者最后都会用JavaScript来包装`<div>`中的每一行，这样它们可以和`:nth-child()`技术一样，在语法高亮中使用这种方法。这不仅是它减少了理论简洁的原因（JS不应该用于样式），还因为很多DOM元素会拖慢页面的加载，所以这是一个非常脆弱的解决方案（当你增加文字大小，还有某个换行时会怎样呢？）。还有更好的方法吗？

> 很多作者甚至给 css 工作小组发了请求，申请一个`:nth-line()`伪类，但是因为性能原因被否决了。

## 解决方案

除了伪元素应用一个较暗的背景颜色来代表行，我们以另一个角度来思考问题。为什么不可以为整个元素应用一个带斑马条纹的背景图像呢？这刚开始听起来可能比较可怕，但是我们可以直接在CSS中生成背景，通过CSS渐变并让它们以`em`为单位，这样它们可以自动适应`font-size`的变化。

让我们延伸一下这个想法，写出下图中的斑马条纹的代码。

![斑马条纹背景的文本行](https://www.w3cplus.com/sites/default/files/blogs/2016/1601/css-secrets-5-9.png)

首先，我们需要创建横条纹，根据《[CSS秘密花园：条纹背景](http://www.w3cplus.com/css3/css-secrets/striped-backgrounds.html)》方案。`background-size`需要是`line-height`的两倍，因为每个条占两行。我们第一次尝试的代码如下：

> 因为一行带色一行不带色，所以需要一个背景大小等于两行的背景，然后通过线性渐变，上下颜色渐变，即可完成需求。

```css
padding: .5em;
line-height: 1.5;
background: beige;
background-image: linear-gradient(
                  rgba(0,0,0,.2) 50%, transparent 0);
background-size: auto 3em;
```

![斑马条纹背景的文本行](https://www.w3cplus.com/sites/default/files/blogs/2016/1601/css-secrets-5-10.png)

结果和我们想要的是非常相近的。我们可以尝试改变字体大小，还有条纹收缩或扩张成我们需要的大小！但是，有一个很严重的问题：线条没有对齐，和我们的目的不符。这是为什么？

如果你仔细看看上图，你会发现第一个条纹是从我们容器的顶部开始的，正如我们希望从背景图像中得到的一样。但是，我们的代码不会从那里开始，因为这样看起来会非常丑。所以正如你看到的，我们应用了一个`.5em`的内边距，这使得它和我们的条纹偏移了。

解决这个问题的一种方法是使用`background-position`来让条纹相对于底部移动`.5em`。但是，如果我们决定后边调整内边距，我们还需要调整背景位置，这并不是非常DRY。我们可以让背景自动适应`padding`的长度吗？

我们回忆一下`background-origin`。这正是我们需要的：一个告诉浏览器使用内容框边缘作为基准来解决`background-position`的方法，而不是默认的填充框边缘。让我们把它添加到前边的代码中：

```css
padding: .5em;
line-height: 1.5;
background: beige;
background-size: auto 3em;
background-origin: content-box;
background-image: linear-gradient(rgba(0,0,0,.2) 50%,
                  transparent 0);
```

![斑马条纹背景的文本行](https://www.w3cplus.com/sites/default/files/blogs/2016/1601/css-secrets-5-11.png)

这正是我们想要完成的斑马条纹效果的代码！因为我们在条纹中使用了半透明背景的颜色，我们可以调整背景颜色，斑马条纹仍然可以工作。基本上，这是非常灵活的，破坏它的唯一途径是改变`line-height`的值，而没有相应改变`background-size`的值。

这是假设我们正在处理代码片段。在一般情况下，如果有内联元素需要一个更大的行高的时候，它也可以被打破，比如有较大`fontsize`的图片或内联内容。



转载：https://www.w3cplus.com/css3/css-secrets/zebra-strlped-text-lines.html[w3cplus.com](https://www.w3cplus.com/)