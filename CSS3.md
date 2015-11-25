# CSS3 介绍

CSS3 是最新的 CSS 标准。

CSS3 完全向后兼容低版本的 CSS。

这章节讲解关于 CSS3 的新功能。

## CSS3 Modules

CSS3 已经被分割成模块，它包含旧的 CSS 规范 (也被分成更小的部分)。除此，新的模块也已被添加。

一些 CSS3 中最重要的模块：

* Selectors
* Box Model
* Backgrounds and Borders
* Image Values and Replaced Content
* Text Effects
* 2D/3D Transformations
* Animations
* Multiple Column Layout
* User Interface

## CSS3 Recommendation

Most of the CSS3 Modules are W3C Recommendations, and most of the new CSS3 properties are already implemented in modern browsers.

# CSS3 角落圆角

使用 `border-radius`，你可以对任意元素指定 “圆角” 样式。 

> **Tip:** The `border-radius` property is actually a shorthand property for the `border-top-left-radius`, `border-top-right-radius`, `border-bottom-right-radius` and `border-bottom-left-radius` properties.

## CSS3 border-radius - Specify Each Corner

If you specify only one value for the border-radius property, this radius will be applied to all 4 corners.

## CSS3 Rounded Corners Properties
Property	 | Description
------------- | -------------
border-radius	 | A shorthand property for setting all the four `border-*-*-radius` properties
border-top-left-radius | 	Defines the shape of the border of the top-left corner
border-top-right-radius	 | Defines the shape of the border of the top-right corner
border-bottom-right-radius	 | Defines the shape of the border of the bottom-right corner
border-bottom-left-radius | 	Defines the shape of the border of the bottom-left corner

# CSS3 边框图像

使用 border-image 属性，你可以设置一张图像作为环绕元素的边框。

## 浏览器支持

Property | Chrome | Edge | IE | FireFox | Safari | Opear
------------- | ------------- | ------------- | ------------- | ------------- | ------------- | -------------					
border-image	 | 16.0  4.0 -webkit- | 	12.0	11.0  |  15.0 3.5 -moz- | 	6.0 3.1 -webkit- | 	15.0 11.0 -o-


## CSS3 border-image Property

`border-image` 属性有三部分：

1.用作边框的图像
2.从哪裁剪图像
3.定义的中间部分应该重复还是拉伸。

```css
#borderimg {
    border: 10px solid transparent;
    padding: 15px;
    -webkit-border-image: url(border.png) 30 stretch; /* Safari 3.1-5 */
    -o-border-image: url(border.png) 30 stretch; /* Opera 11-12.1 */
    border-image: url(border.png) 30 stretch;
}
```

注意，要使 border-image 有效，border 必须设置。


> **Tip:** The **border-image** property is actually a shorthand property for the **border-image-source**, **border-image-slice**, **border-image-width**, **border-image-outset** and **border-image-repeat** properties.



## CSS3 Border Properties


Property | 	Description
------------- | -------------
border-image | 	A shorthand property for setting all the border-image-* properties
border-image-source | 	Specifies the path to the image to be used as a border
border-image-slice | 	Specifies how to slice the border image
border-image-width | 	Specifies the widths of the border image
border-image-outset | 	Specifies the amount by which the border image area extends beyond the border box
border-image-repeat | 	Specifies whether the border image should be repeated, rounded or stretched

# CSS3 Backgrounds

CSS3 包含少量新的 background 属性，它使我们更好的控制 background element。

在这个章节你会学习到如何添加多个背景图片到单个 element。

你也将会学到下面的新 CSS3 properties：

* `background-size`
* `background-origin`
* `background-clip`

## Browser Support

[这里](http://www.w3schools.com/css/css3_backgrounds.asp)。

## CSS3 Multiple Backgrounds

CSS3 允许你添加多个背景图像给单个元素，通过 `background-image` 属性。

不同的 background 由 `,`分隔，图像会堆叠到另一张的顶部，第一张图像最接近观察者。

下面的例子有两个背景图像，第一张图像是花 (对齐右边底部)，第二张图像是纸张背景 (对齐左上角)。

```css
#example1 {
    background-image: url(img_flwr.gif), url(paper.gif);
    background-position: right bottom, left top;
    background-repeat: no-repeat, repeat;
}
```
[link](http://www.w3schools.com/css/tryit.asp?filename=trycss3_background_multiple)。

Multiple background images can be specified using either the individual background properties (as above) or the `background` shorthand property.

The following example uses the `background` shorthand property (same result as example above):

```css
#example1 {
    background: url(img_flwr.gif) right bottom no-repeat, url(paper.gif) left top repeat;
}
```

## CSS3 Background Size

CSS3 `background-size` 属性允许你指定背景图像的尺寸。

在 CSS3 之前，背景图像的尺寸是图像的实际尺寸。CSS3 允许我们在不同的上下文重用背景图像。

尺寸可以指定为长度，百分比，或使用这两个关键字：contain or cover。

The following example resizes a background image to much smaller than the original image (using pixels):

Original background image:

```css
#example1 {
    border: 1px solid black;
    background:url(img_flwr.gif);
    background-repeat: no-repeat;
    padding:15px;
}

#example2 {
    border: 1px solid black;
    background:url(img_flwr.gif);
    background-size: 100px 80px;
    background-repeat: no-repeat;
    padding:15px;
}
```

The two other possible values for `background-size` are `contain` and `cover`.

`contain` 关键字会尽可能大的缩放背景图像 (但它的宽度和高度必须适应 content area)。因此，取决于 background iamge 属性和 baockground positioning 区域，元素的背景可能有一些区域不会被 background image 覆盖。

`cover` 关键字缩放背景图像使 content area 完全被 background image 覆盖 (它的宽度和高度会等于或超过 content area)。因此，background image 的某些部分可能在 background positioning 区域不可见。

下面的例子说明 `contain` 和 `cover` 的使用：

```css
#div1 {
    background: url(img_flower.jpg);
    background-size: contain;
    background-repeat: no-repeat;
}
#div2 {
    background: url(img_flower.jpg);
    background-size: cover;
    background-repeat: no-repeat;
}
```

## Define Sizes of Multiple Background Images

`background-size` 属性也接收多个属性 (用 `,` 分隔)，当有多个 background image 时。

The following example has three background images specified, with different background-size value for each image:

```css
#example1 {
    background: url(img_flwr.gif) left top no-repeat, url(img_flwr.gif) right bottom no-repeat, url(paper.gif) left top repeat;
    background-size: 50px, 130px, auto;
}
```

## Full Size Background Image

现在我们有一个背景图像，我们像让它覆盖整个浏览器窗口在所有时候：

需要做下面的事情：

* 填充整个图像到页面 (无空白空间)
* 根据需要拉伸图像
* 使图像位于页面中间
* 不响应滚动栏

下面的例子显示如何完成，使用了 html 元素 (html 元素总是浏览器窗口的最小高度)。然后设置固定和中心的背景。然后调整 `backgroun-size` 属性的尺寸：

```css
html {
    background: url(img_flower.jpg) no-repeat center center fixed; 
    background-size: cover;
}
```

## CSS3 background-origin Property

CSS3 `backgroun-origin` 属性指定 `background image` 在哪定位。

该属性有三个不同的值：

* border-box:background image 从边框的左上角开始。
* padding-box:默认值，background image padding edge 的左上角开始。
* content-box:background image 从 content 的左上角开始。

下面是说明 `background-origin` 属性的例子：

```css
#example1 {
    border: 10px solid black;
    padding:35px;
    background:url(img_flwr.gif);
    background-repeat: no-repeat;
    background-origin: content-box;
}
```

## CSS3 background-clip Property

CSS3 `background-clip` 属性指定 background 的绘制区域。

该属性有三个不同的值：

* border-box:默认值，background 从边框的外边缘开始绘制。
* padding-box:background 从 padding 的外边缘开始绘制。
* content-box:background 在 content box 内绘制。

The following example illustrates the `background-clip` property:

```css
#example1 {
    border: 10px dotted black;
    padding:35px;
    background: yellow;
    background-clip: content-box;
}
```

## CSS3 Background Properties

Property | 	Description
------------- | -------------
background | 	A shorthand property for setting all the background properties in one declaration
background-clip | 	Specifies the painting area of the background
background-image | 	Specifies one or more background images for an element
background-origin | 	Specifies where the background image(s) is/are positioned
background-size | 	Specifies the size of the background image(s)

# CSS3 Colors

CSS 支持 color name，hexadecimal and RGB color。

In addition, CSS3 also introduces:

* RGBA colors
* HSL colors
* HSLA colors
* opacity

## Borwser Support

[link](http://www.w3schools.com/css/css3_colors.asp)


## RGBA Colors

RGBA color values are an extension of RGB color values with an alpha channel - which specifies the opacity for a color.

An RGBA color value is specified with: rgba(red, green, blue, alpha). The alpha parameter is a number between 0.0 (fully transparent) and 1.0 (fully opaque).

```css
rgba(255, 0, 0, 0.2);
rgba(255, 0, 0, 0.4);
rgba(255, 0, 0, 0.6);
rgba(255, 0, 0, 0.8);

#p1 {background-color: rgba(255, 0, 0, 0.3);}  /* red with opacity */
#p2 {background-color: rgba(0, 255, 0, 0.3);}  /* green with opacity */
#p3 {background-color: rgba(0, 0, 255, 0.3);}  /* blue with opacity */

```


## HSL Colors

HSL 表示色调，饱和，和亮度。

An HSL color value is specified with: hsl(hue, saturation, lightness).

1.色调是色轮上的度数 (0 到 360)

* 0 (or 360) is red
* 120 is green
* 240 is blue

2.饱和是百分比值：100% 是完整的颜色。
3.亮度也是百分比，0% 是灰(黑) 和 100% 是白色。

The following example defines different HSL colors:

```css
#p1 {background-color: hsl(120, 100%, 50%);}  /* green */
#p2 {background-color: hsl(120, 100%, 75%);}  /* light green */
#p3 {background-color: hsl(120, 100%, 25%);}  /* dark green */
#p4 {background-color: hsl(120, 60%, 70%);}   /* pastel green */
```

## HSLA Colors

HSLA 是带有透明度参数的 HSL color 拓展。

An HSLA color value is specified with: hsla(hue, saturation, lightness, alpha), where the alpha parameter defines the opacity. The alpha parameter is a number between 0.0 (fully transparent) and 1.0 (fully opaque).

```css
#p1 {background-color: hsla(120, 100%, 50%, 0.3);}  /* green with opacity */
#p2 {background-color: hsla(120, 100%, 75%, 0.3);}  /* light green with opacity */
#p3 {background-color: hsla(120, 100%, 25%, 0.3);}  /* dark green with opacity */
#p4 {background-color: hsla(120, 60%, 70%, 0.3);}   /* pastel green with opacity */
```

## Opacity

The CSS3 opacity property sets the opacity for a specified RGB value.

The opacity property value must be a number between 0.0 (fully transparent) and 1.0 (fully opaque).

```css
#p1 {background-color:rgb(255,0,0);opacity:0.6;}  /* red with opacity */
#p2 {background-color:rgb(0,255,0);opacity:0.6;}  /* green with opacity */
#p3 {background-color:rgb(0,0,255);opacity:0.6;}  /* blue with opacity */
```

# CSS3 Gradients

CSS3 渐变让你在两个或更多颜色之间显示光滑过度的效果。

在早期，你会使用图像代替这些效果。现在，通过使用 CSS3 gradients 你可以缩减网站下载时间和宽带使用情况。此外，元素在缩放时效果更好，因为渐变效果是由浏览器生成。

CSS3 定义了两种类型的渐变:

* **行渐变((goes down/up/left/right/diagonally)**
* **迳向渐变 (通过它的中心定义)**

## Browser Support

[link](http://www.w3schools.com/css/css3_gradients.asp)。

## CSS3 Linear Gradients

要创建 linear gradient 你必须定义至少两个 color stops。Color stop 是你想要渲染的平滑过渡之间的颜色。你也可以设置 starting point 和过渡效果的方向 (或角度)。

### 语法

```css
background: linear-gradient(direction, color-stop1, color-stop2, ...);
```

### Linear Gradient - Top to Bottom (this is default)

```css
#grad {
  background: -webkit-linear-gradient(red, blue); /* For Safari 5.1 to 6.0 */
  background: -o-linear-gradient(red, blue); /* For Opera 11.1 to 12.0 */
  background: -moz-linear-gradient(red, blue); /* For Firefox 3.6 to 15 */
  background: linear-gradient(red, blue); /* Standard syntax */
}
```

### Linear Gradient - Left to Right

The following example shows a linear gradient that starts from the left. It starts red, transitioning to blue:

```css
#grad {
  background: -webkit-linear-gradient(left, red , blue); /* For Safari 5.1 to 6.0 */
  background: -o-linear-gradient(right, red, blue); /* For Opera 11.1 to 12.0 */
  background: -moz-linear-gradient(right, red, blue); /* For Firefox 3.6 to 15 */
  background: linear-gradient(to right, red , blue); /* Standard syntax */
}
```

### Linear Gradient - Diagonal

You can make a gradient diagonally by specifying both the horizontal and vertical starting positions.

The following example shows a linear gradient that starts at top left (and goes to bottom right). It starts red, transitioning to blue:

```css
#grad {
  background: -webkit-linear-gradient(left top, red , blue); /* For Safari 5.1 to 6.0 */
  background: -o-linear-gradient(bottom right, red, blue); /* For Opera 11.1 to 12.0 */
  background: -moz-linear-gradient(bottom right, red, blue); /* For Firefox 3.6 to 15 */
  background: linear-gradient(to bottom right, red , blue); /* Standard syntax */
}
```

## Using Angles

If you want more control over the direction of the gradient, you can define an angle, instead of the predefined directions (to bottom, to top, to right, to left, to bottom right, etc.).

### Syntax

```css
background: linear-gradient(angle, color-stop1, color-stop2);
```

angle 指水平线和渐变线条之间，顺时针的角度。

The following example shows how to use angles on linear gradients:

```css
#grad {
  background: -webkit-linear-gradient(180deg, red, blue); /* For Safari 5.1 to 6.0 */
  background: -o-linear-gradient(180deg, red, blue); /* For Opera 11.1 to 12.0 */
  background: -moz-linear-gradient(180deg, red, blue); /* For Firefox 3.6 to 15 */
  background: linear-gradient(180deg, red, blue); /* Standard syntax */
}
```


## Using Multiple Color Stops

The following example shows how to set multiple color stops:

```css
#grad {
  background: -webkit-linear-gradient(red, green, blue); /* For Safari 5.1 to 6.0 */
  background: -o-linear-gradient(red, green, blue); /* For Opera 11.1 to 12.0 */
  background: -moz-linear-gradient(red, green, blue); /* For Firefox 3.6 to 15 */
  background: linear-gradient(red, green, blue); /* Standard syntax */
}
```

下面的例子演示如何创建 linear gradient 为彩虹颜色并带上一些文本：

```css
#grad {
  /* For Safari 5.1 to 6.0 */
  background: -webkit-linear-gradient(left,red,orange,yellow,green,blue,indigo,violet);
  /* For Opera 11.1 to 12.0 */
  background: -o-linear-gradient(left,red,orange,yellow,green,blue,indigo,violet);
  /* For Fx 3.6 to 15 */
  background: -moz-linear-gradient(left,red,orange,yellow,green,blue,indigo,violet);
  /* Standard syntax */
  background: linear-gradient(to right, red,orange,yellow,green,blue,indigo,violet); 
}
```

## Using Transparency

CSS3 gradients 也支持使用透明度，可以用来创建变淡效果。

要添加透明度，我们使用 rgba() 函数。

The following example shows a linear gradient that starts from the left. It starts fully transparent, transitioning to full color red:

```css
#grad {
  background: -webkit-linear-gradient(left,rgba(255,0,0,0),rgba(255,0,0,1)); /*Safari 5.1-6*/
  background: -o-linear-gradient(right,rgba(255,0,0,0),rgba(255,0,0,1)); /*Opera 11.1-12*/
  background: -moz-linear-gradient(right,rgba(255,0,0,0),rgba(255,0,0,1)); /*Fx 3.6-15*/
  background: linear-gradient(to right, rgba(255,0,0,0), rgba(255,0,0,1)); /*Standard*/
}
```

## Repeating a linear-gradient

`repeating-linear-gradient()` 函数用作重复 linear gradients。

```css
#grad {
  /* Safari 5.1 to 6.0 */
  background: -webkit-repeating-linear-gradient(red, yellow 10%, green 20%);
  /* Opera 11.1 to 12.0 */
  background: -o-repeating-linear-gradient(red, yellow 10%, green 20%);
  /* Firefox 3.6 to 15 */
  background: -moz-repeating-linear-gradient(red, yellow 10%, green 20%);
  /* Standard syntax */
  background: repeating-linear-gradient(red, yellow 10%, green 20%);
}

```

## CSS3 Radial Gradients

radial gradient 是通过它的中心定义。

要创建 radial gradient 你必须也定义至少两个 color stop。

### 语法

```css
background: radial-gradient(shape size at position, start-color, ..., last-color);
```

形状默认是 ellipse，尺寸是 farthest-corner，位置是 center

### Radial Gradient - Evenly Spaced Color Stops (this is default)

```css
#grad {
  background: -webkit-radial-gradient(red, green, blue); /* Safari 5.1 to 6.0 */
  background: -o-radial-gradient(red, green, blue); /* For Opera 11.6 to 12.0 */
  background: -moz-radial-gradient(red, green, blue); /* For Firefox 3.6 to 15 */
  background: radial-gradient(red, green, blue); /* Standard syntax */
}
```

### Radial Gradient - Differently Spaced Color Stops

```css
#grad {
  background: -webkit-radial-gradient(red 5%, green 15%, blue 60%); /* Safari 5.1-6.0 */
  background: -o-radial-gradient(red 5%, green 15%, blue 60%); /* For Opera 11.6-12.0 */
  background: -moz-radial-gradient(red 5%, green 15%, blue 60%); /* For Firefox 3.6-15 */
  background: radial-gradient(red 5%, green 15%, blue 60%); /* Standard syntax */
}
```

## Set Shape

shape 参数用来定义 shape，它可以是 circle 或 ellipse，默认值是 ellipse。

```css
#grad {
  background: -webkit-radial-gradient(circle, red, yellow, green); /* Safari */
  background: -o-radial-gradient(circle, red, yellow, green); /* Opera 11.6 to 12.0 */
  background: -moz-radial-gradient(circle, red, yellow, green); /* Firefox 3.6 to 15 */
  background: radial-gradient(circle, red, yellow, green); /* Standard syntax */
}
```

## Use of Different Size Keywords

The size parameter defines the size of the gradient. It can take four values:

* closest-side
* farthest-side
* closest-corner
* farthest-corner

```css
#grad1 {
  /* Safari 5.1 to 6.0 */
  background: -webkit-radial-gradient(60% 55%, closest-side,blue,green,yellow,black); 
  /* For Opera 11.6 to 12.0 */
  background: -o-radial-gradient(60% 55%, closest-side,blue,green,yellow,black);
  /* For Firefox 3.6 to 15 */
  background: -moz-radial-gradient(60% 55%, closest-side,blue,green,yellow,black);
  /* Standard syntax */
  background: radial-gradient(closest-side at 60% 55%,blue,green,yellow,black);
}

#grad2 {
  /* Safari 5.1 to 6.0 */
  background: -webkit-radial-gradient(60% 55%, farthest-side,blue,green,yellow,black);
  /* Opera 11.6 to 12.0 */ 
  background: -o-radial-gradient(60% 55%, farthest-side,blue,green,yellow,black);
  /* For Firefox 3.6 to 15 */
  background: -moz-radial-gradient(60% 55%, farthest-side,blue,green,yellow,black);
  /* Standard syntax */
  background: radial-gradient(farthest-side at 60% 55%,blue,green,yellow,black);
}
```

## Repeating a radial-gradient

```css
#grad {
  /* For Safari 5.1 to 6.0 */
  background: -webkit-repeating-radial-gradient(red, yellow 10%, green 15%);
  /* For Opera 11.6 to 12.0 */
  background: -o-repeating-radial-gradient(red, yellow 10%, green 15%);
  /* For Firefox 3.6 to 15 */
  background: -moz-repeating-radial-gradient(red, yellow 10%, green 15%);
  /* Standard syntax */
  background: repeating-radial-gradient(red, yellow 10%, green 15%);
}
```


# CSS3 Shadow Effects

在 CSS3，你可以对文字和元素添加阴影效果。

这个章节中你将会学习下面两个属性：

* `text-shadow`
* `box-shadow`

## Browser Support

[link](http://www.w3schools.com/css/css3_shadows.asp)。

## CSS3 Text Shadow

`text-shadow` 属性对文本应用阴影。

下面是最简单的使用方式，只需要设定水平阴影和垂直阴影：

```css
h1 {
    text-shadow: 2px 2px;
}
```

接着，对阴影添加一些颜色：

```css
h1 {
    text-shadow: 2px 2px red;
}
```

在添加一些模糊效果：

```css
h1 {
    text-shadow: 2px 2px 5px red;
}
```


霓虹灯光阴影

```css
h1 {
    text-shadow: 0 0 3px #FF0000;
}
```

### Multiple Shadows

可以对文本添加一个以上的阴影，通过 `,` 分隔阴影属性列。

下面演示红色和蓝色的霓虹灯阴影：

```css
h1 {
    text-shadow: 0 0 3px #FF0000, 0 0 5px #0000FF;
}
```

The following example shows a white text with black, blue, and darkblue shadow:

```css
h1 {
    color: white;
    text-shadow: 1px 1px 2px black, 0 0 25px blue, 0 0 5px darkblue;
}
```


## CSS3 box-shadow Property

CSS3 `box-shadow` 属性对元素应用阴影。

下面是最简单的使用，只需要指定水平阴影和垂直阴影：

```css
div {
    box-shadow: 10px 10px;
}
```

对阴影添加颜色：

```css
div {
    box-shadow: 10px 10px grey;
}
```

对阴影添加模糊：

```css
div {
    box-shadow: 10px 10px 5px grey;
}
```


## CSS3 Shadow Properties

Property | 	Description
------------- | -------------
[box-shadow](http://www.w3schools.com/cssref/css3_pr_box-shadow.asp) | 	Adds one or more shadows to an element
[text-shadow](http://www.w3schools.com/cssref/css3_pr_text-shadow.asp) | 	Adds one or more shadows to a text


# CSS3 Text

CSS3 contains several new text features.

In this chapter you will learn about the following text properties:

* `text-overflow`
* `word-wrap`
* `word-break`

## Browser Support

[Link](http://www.w3schools.com/css/css3_text_effects.asp)。

`text-overflow` 属性指定对溢出的文本如何对用户表达。

它可以是 clipped，或 ellipsis (...)。

下面的例子演示如何在鼠标移动到元素上时显示溢出的文本内容：

```css
div.test:hover {
    text-overflow: inherit;
    overflow: visible;
}
```

## CSS3 Word Wrapping

`word-wrap` 属性允许过长的文本自动换行到下一行。

```css
p {
    word-wrap: break-word;
}
```

## CSS3 Word Breaking

`word-break` 指定中断行的规则。

单词不断行与单词换行。

```css
p.test1 {
    word-break: keep-all;
}

p.test2 {
    word-break: break-all;
}
```

# CSS3 Web Fonts

Web front 允许网页设计者使用未安装在用户计算器上的字体。

当你 寻找到/购买到 你希望使用的字体时，只需要包含字体文件到你的 web 服务器，当用户需要时它将会被自动下载。

“属于你” 的字体定义在 CSS3 `@font-face` 规则。

## Browser Support

[Link](http://www.w3schools.com/css/css3_fonts.asp)。

## Different Font Formats

### TrueType Fonts (TTF)

TrueType is a font standard developed in the late 1980s, by Apple and Microsoft. TrueType is the most common font format for both the Mac OS and Microsoft Windows operating systems.

### OpenType Fonts (OTF)

OpenType is a format for scalable computer fonts. It was built on TrueType, and is a registered trademark of Microsoft. OpenType fonts are used commonly today on the major computer platforms.

### The Web Open Font Format (WOFF)

WOFF is a font format for use in web pages. It was developed in 2009, and is now a W3C Recommendation. WOFF is essentially OpenType or TrueType with compression and additional metadata. The goal is to support font distribution from a server to a client over a network with bandwidth constraints.

### The Web Open Font Format (WOFF 2.0)

TrueType/OpenType font that provides better compression than WOFF 1.0.

### SVG Fonts/Shapes

SVG fonts allow SVG to be used as glyphs when displaying text. The SVG 1.1 specification define a font module that allows the creation of fonts within an SVG document. You can also apply CSS to SVG documents, and the @font-face rule can be applied to text in SVG documents.

### Embedded OpenType Fonts (EOT)

EOT fonts are a compact form of OpenType fonts designed by Microsoft for use as embedded fonts on web pages.

## Browser Support for Font Formats

[Link](http://www.w3schools.com/css/css3_fonts.asp)。

## Using The Font You Want

在 CSS3 `@font-face` 的规则，你必须首先定义 font 的名称，然后指向 font 文件。

> **提示：**总是对 URL 使用小写字母。大写字母在 IE 中会出现意外的结果。

要对 HTML 元素使用 font，把 font 的名称传递给 `font-family` 属性。

```css
@font-face {
    font-family: myFirstFont;
    src: url(sansation_light.woff);
}

div {
    font-family: myFirstFont;
}
```

## Using Bold Text

使用了自定义的 `@font-face` 字体后，你必须另外指定 `@font-face` 规则包含 bold text 的描述。

否则将无法使用 `<b>`

```css
@font-face {
   font-family: myFirstFont;
   src: url(sansation_light.woff);
}

@font-face {
   font-family: myFirstFont;
   src: url(sansation_bold.woff);
   font-weight: bold;
}
```

对于同一个字体你可以有多个 `@font-face` 规则。

## CSS3 Font Descriptors

[Link](http://www.w3schools.com/css/css3_fonts.asp)。


# CSS3 2D 转换

CSS3 的 transforms 允许你移动，旋转，拉伸，和倾斜元素。

transformation 会影响改变元素的形状，尺寸和位置。

CSS3 支持 2D 和 3D 转换。

## Browser Support for 2D Transforms

[link](http://www.w3schools.com/css/css3_2dtransforms.asp)


## CSS3 2D Transforms

* `translate()`
* `rotate()`
* `scale()`
* `skewX()`
* `skewY()`
* `matrix()`

### translate() 方法

`translate()` 方法从它当前位置移动元素 (根据给定的 X 坐标和 Y 坐标)。

```css
div {
    -ms-transform: translate(50px,100px); /* IE 9 */
   	-webkit-transform: translate(50px,100px); /* Safari */
    transform: translate(50px,100px);
}
```

### rotate() 方法

`rotate()` 方法根据给定的角度顺时针或逆时针旋转元素。

```css
div {
    -ms-transform: rotate(20deg); /* IE 9 */
    -webkit-transform: rotate(20deg); /* Safari */
    transform: rotate(20deg);
}
```

使用负值逆时针旋转元素

```css
div {
    -ms-transform: rotate(-20deg); /* IE 9 */
    -webkit-transform: rotate(-20deg); /* Safari */
    transform: rotate(-20deg);
}
```

### scale() 方法

`scale()` 方法提高或缩减元素的尺寸，根据给定的 width 和 height。

下面的例子提升 <div> 元素的为原始宽度的两倍，和元素高度的三倍：

```css
div {
    -ms-transform: scale(2,3); /* IE 9 */
    -webkit-transform: scale(2,3); /* Safari */
    transform: scale(2,3);
}
```

使用小数缩减元素的尺寸：

```css
div {
    -ms-transform: scale(0.5,0.5); /* IE 9 */
    -webkit-transform: scale(0.5,0.5); /* Safari */
    transform: scale(0.5,0.5);
}
```

### skewX() 方法

使用 `skewX()` 使元素的 X 轴倾斜。

下面的例子使元素倾斜 20 度。

```css
div {
    -ms-transform: skewX(20deg); /* IE 9 */
    -webkit-transform: skewX(20deg); /* Safari */
    transform: skewX(20deg);
}
```

### skewY() 方法

skewY() 方法根据给定的角度使元素的 Y 轴倾斜。

```css
div {
    -ms-transform: skewY(20deg); /* IE 9 */
    -webkit-transform: skewY(20deg); /* Safari */
    transform: skewY(20deg);
}
```

### skew() 方法

`skew()` 方法使元素的 X 轴和 Y 轴倾斜。

```css
div {
    -ms-transform: skew(20deg, 10deg); /* IE 9 */
    -webkit-transform: skew(20deg, 10deg); /* Safari */
    transform: skew(20deg, 10deg);
}
```

如果第二个参数未指定，那么它的值是 0。所以下面的例子只使 X 轴倾斜 20 度。

```css
div {
    -ms-transform: skew(20deg); /* IE 9 */
    -webkit-transform: skew(20deg); /* Safari */
    transform: skew(20deg);
}
```

### matrix() 方法

matrix() 方法组合所有 2D transform 方法到一个中。

matrix() 方法需要 6 个参数，里面包含的数学函数允许你的 旋转，拉伸，移动 (偏移)，和倾斜元素 

```css
div {
    -ms-transform: matrix(1, -0.3, 0, 1, 0, 0); /* IE 9 */
    -webkit-transform: matrix(1, -0.3, 0, 1, 0, 0); /* Safari */
    transform: matrix(1, -0.3, 0, 1, 0, 0);
}
```

# CSS3 3D 转换

CSS3 允许你的使用 3D transformations 你的元素。

## Browser Support for 3D Transforms

[link](http://www.w3schools.com/css/css3_3dtransforms.asp)。

## CSS3 3D Transform

在这个章节将会学习三个 3D transformation 方法。

* `rotateX()`
* `rotateY()`
* `rotateZ()`

### rotateX() 方法

`rotateX()` 方法使元素环绕它的 X 轴旋转给定的角度。

```css
div {
    -webkit-transform: rotateX(150deg); /* Safari */
    transform: rotateX(150deg);
}
```

### rotateY() 方法

`rotateY()` 方法使元素环绕它的 Y 轴旋转给定的角度。

```css
div {
    -webkit-transform: rotateY(130deg); /* Safari */
    transform: rotateY(130deg);
}
```

### rotateZ() 方法

`rotateZ()` 方法使元素围绕它的 Z 轴旋转给定的角度。

```css
div {
    -webkit-transform: rotateZ(90deg); /* Safari */
    transform: rotateZ(90deg);
}
```

## CSS3 转换属性

Property	Description
[transform](http://www.w3schools.com/cssref/css3_pr_transform.asp)	对元素应用 2D 或 3D 转换方法。
[transform-origin](http://www.w3schools.com/cssref/css3_pr_transform-origin.asp)	允许你更改已转换的元素的位置
[transform-style](http://www.w3schools.com/cssref/css3_pr_transform-style.asp)	指定嵌套的元素如何在 3D 空间渲染
[perspective](http://www.w3schools.com/cssref/css3_pr_perspective.asp) 指定可见的 3D 元素视角
[perspective-origin](http://www.w3schools.com/cssref/css3_pr_perspective-origin.asp)	指定 3D 元素的底部位置
[backface-visibility](http://www.w3schools.com/cssref/css3_pr_backface-visibility.asp)	指定当 3D 元素不对着屏幕时是否可见

# CSS3 Transitions

CSS3 的 Transition 允许你更高属性的值，在给定的时间平滑过渡。

## Browser Support for Transitions

[link](http://www.w3schools.com/css/css3_transitions.asp)。

## 如何使用 CSS3 Transition

要创建 transition 效果，你必须指定两件事：

* 你想要添加效果的 CSS 属性
* 效果执行的持续时间

**注意：** 如果持续时间未指定，transition 将不会有效果，因为默认值是 0。

下面的例子显示一个 100px * 100px 的红色 <div> 元素。这个 <div> 元素为 width 指定了 transition 效果，持续两秒的时间。

```css
div {
    width: 100px;
    height: 100px;
    background: red;
    -webkit-transition: width 2s; /* Safari */
    transition: width 2s;
}
```

当指定的 CSS 属性 (width) 发生改变时，会产生 transition 效果。

例如，下面鼠标移动到元素上更改 width 值：

```css
div:hover {
    width: 300px;
}
```

## 更改多个属性值

下面的例子同时对元素的 width 和 height 属性添加 transition 效果。宽度是 2 秒的持续时间，高度是 4秒。

```css
div {
    -webkit-transition: width 2s, height 4s; /* Safari */
    transition: width 2s, height 4s;
}
```

## 指定 Transition 的速度曲线

`transition-timing-function` 属性指定 transition 效果的速度曲线。

transition-timing-function 属性可以有以下的值：

* `ease` - specifies a transition effect with a slow start, then fast, then end slowly (this is default)
* `linear` - specifies a transition effect with the same speed from start to end
* `ease-in` - specifies a transition effect with a slow start
* `ease-out` - specifies a transition effect with a slow end
* `ease-in-out` - specifies a transition effect with a slow start and end
* `cubic-bezier(n,n,n,n)` - lets you define your own values in a cubic-bezier function

```css
#div1 {transition-timing-function: linear;}
#div2 {transition-timing-function: ease;}
#div3 {transition-timing-function: ease-in;}
#div4 {transition-timing-function: ease-out;}
#div5 {transition-timing-function: ease-in-out;}
```

## 延迟 transition 效果

`transition-delay` 属性指定 transition 效果的延迟时间 (秒)。

下面的例子指定 transition 效果开始之前有 1 秒延时：

```css
div {
    -webkit-transition-delay: 1s; /* Safari */
    transition-delay: 1s;
}
```

## Transition + Transformation

下面的例子将转换效果配合 transition 使用：

```css
div {
    -webkit-transition: width 2s, height 2s, -webkit-transform 2s; /* Safari */
    transition: width 2s, height 2s, transform 2s;
}
```


## More Transition Examples

The CSS3 transition properties can be specified one by one, like this:

```css
div {
    transition-property: width;
    transition-duration: 2s;
    transition-timing-function: linear;
    transition-delay: 1s;
}
```

# CSS3 Animations

CSS3 的 animation 允许你对大部分 HTML 元素动画而不需要使用 JavaScript 或 Flash。

## Browser Support for Animations

[link](http://www.w3schools.com/css/css3_animations.asp)。

## 什么是 CSS3 Animation？

animation 让元素逐渐的从一个样式更改到另一个。

你可以更改许多你想要的 CSS 属性。

要使用 CSS3 animation，你必须首先指定一些 keyframes 给 animation。

keyframes 保存着元素在某个时间内的样式。

## @keyframe rule

当你在 `@keyframe` 内指定 CSS 样式时，animation 会在一段时间内逐渐从当前样式更改到新的样式。

要使 animation 工作，你必须绑定 animation 到一个元素。

下面的例子绑定一个 "example" 元素到 <div> 元素。animation 会持续 4 秒，逐渐的更改 <div> 元素的背景颜色从红到黄。

```css
/* The animation code */
@keyframes example {
    from {background-color: red;}
    to {background-color: yellow;}
}

/* The element to apply the animation to */
div {
    width: 100px;
    height: 100px;
    background-color: red;
    animation-name: example;
    animation-duration: 4s;
}
```

## Delay an Animation

The `animation-delay` property specifies a delay for the start of an animation.

The following example has a 2 seconds delay before starting the animation:

```css
div {
    width: 100px;
    height: 100px;
    position: relative;
    background-color: red;
    animation-name: example;
    animation-duration: 4s;
    animation-delay: 2s;
}
```

## 设置 animation 的运行次数

`animation-iteration-count` 属性指定动画效果应该运行的次数。

```css
div {
    width: 100px;
    height: 100px;
    position: relative;
    background-color: red;
    animation-name: example;
    animation-duration: 4s;
    animation-iteration-count: 3;
}
```

下面的例子使用 `infinite` 指定 animation 运行次数无限制。

```css
div {
    width: 100px;
    height: 100px;
    position: relative;
    background-color: red;
    animation-name: example;
    animation-duration: 4s;
    animation-iteration-count: infinite;
}
```

`animation-direction` 属性用作让元素反方向运行或交替循环：

```css
div {
    width: 100px;
    height: 100px;
    position: relative;
    background-color: red;
    animation-name: example;
    animation-duration: 4s;
    animation-iteration-count: 3;
    animation-direction: reverse;
}
```

下面的例子使用 `alternate` 是 animation 实现交替运行。

```css
div {
    width: 100px;
    height: 100px;
    position: relative;
    background-color: red;
    animation-name: example;
    animation-duration: 4s;
    animation-iteration-count: 3;
    animation-direction: alternate;
}
```

## 指定 animation 的动画速度曲线

`animation-timing-function` 用作指定动画的速度曲线。

`animation-timing-function` 属性可以有以下值：

* `ease` - specifies an animation with a slow start, then fast, then end slowly (this is default)
* `linear` - specifies an animation with the same speed from start to end
* `ease-in` - specifies an animation with a slow start
* `ease-out` - specifies an animation with a slow end
* `ease-in-out` - specifies an animation with a slow start and end
* `cubic-bezier(n,n,n,n)` - lets you define your own values in a cubic-bezier function


```css
#div1 {animation-timing-function: linear;}
#div2 {animation-timing-function: ease;}
#div3 {animation-timing-function: ease-in;}
#div4 {animation-timing-function: ease-out;}
#div5 {animation-timing-function: ease-in-out;}
```

## Animation 的缩写属性

下面的例子使用了 6 个动画属性：

```css
div {
    animation-name: example;
    animation-duration: 5s;
    animation-timing-function: linear;
    animation-delay: 2s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
}
```

下面使用了缩写形式实现同样的效果：

```css
div {
    animation: example 5s linear 2s infinite alternate;
}
```


































































































































































































































































































