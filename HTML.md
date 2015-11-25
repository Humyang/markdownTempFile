# HTML



## 介绍

什么是 HTMl？

HTML 是一种描述 web 文档 (web 页面) 的标记语言。

HTML 表示 Hyper Text Markup Language。

HTML document 由 HTML tags 描述。

每个 HTML tag 描述不同的 document 内容。


一个 HTML 小页面。

```html
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>

<h1>This is a Heading</h1>
<p>This is a paragraph.</p>

</body>
</html>

```

解释

**DOCTYPE** 声明 HTML 文档类型的定义。

* `<html>` 和 `</html>` 内的文本描述 HTML document。
* `<head>` 和 `</head>` 之间的文本提供关于 document 的信息。
* `<title>` 和 `</title>` 之间的文本提供文档的 title。
* `<body>` 和 `</body>` 之间的文本描述可见的页面内容。
* `<h1>` 和 `</h1>` 之间的文本描述 heading。
* `<p>` 和 `</p>` 之间的文本描述 paragraph。

使用这些 description，web 浏览器可以显示包含 heading 和 paragraph 的 document。

### The <!DOCTYPE> Declaration

<!DOCTYPE> 帮助浏览器正确的显示 web 页面。

#### HTML5

```html
<!DOCTYPE html>
```

#### HTML 4.01

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

#### XHTML 1.0

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

> All tutorials and examples at W3Schools use HTML5.

### HTML Versions

web 诞生至今，有许多版本的 HTML：

Version	 | Year
------------- | -------------
HTML	 | 1991
HTML 2.0	 | 1995
HTML 3.2	 | 1997
HTML 4.01	 | 1999
XHTML	 | 2000
HTML5	 | 2014


## HTML Attributes

### The lang Attribute

可以在 html 标签指定文档使用的语言，同 lang 属性。

声明语言环境对于应用程序的可访问性 (屏幕阅读者) 和搜索引擎很重要。

`<html lang="en-US">` 前两个字母指定语言，如果有多种方言，用后两个字母区分。

### The title Attribute

HTML paragraphs are defined with the <p> tag.

In this example, the <p> element has a title attribute. The value of the attribute is "About W3Schools":

```html
<p title="About W3Schools">
W3Schools is a web developer's site.
It provides tutorials and references covering
many aspects of web programming,
including HTML, CSS, JavaScript, XML, SQL, PHP, ASP, etc.
</p>
```

### The href Attribute

HTML links are defined with the `<a>` tag. The link address is specified in the href attribute:

```html
<a href="http://www.w3schools.com">This is a link</a>
```

### Size Attributes

HTML images are defined with the `<img>` tag.

The filename of the source (src), and the size of the image (width and height) are all provided as attributes:


```html
<img src="w3schools.jpg" width="104" height="142">
```

### The alt Attribute

The alt attribute specifies an alternative text to be used, when an HTML element cannot be displayed.

The value of the attribute can be read by "screen readers". This way, someone "listening" to the webpage, i.e. a blind person, can "hear" the element.

```html
<img src="w3schools.jpg" alt="W3Schools.com" width="104" height="142">
```

## HTML Headings

### Headings Are Important

Use HTML headings for headings only. Don't use headings to make text **BIG** or **bold**.

