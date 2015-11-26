# React

## 第一个例子

创建一个自己的 Component，只是一个简单的 <div> 元素：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>React Tutorial</title>
    <!-- Not present in the tutorial. Just for basic styling. -->
    <link rel="stylesheet" href="css/base.css" />
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.14.0/react.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/0.14.0/react-dom.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.6.15/browser.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.2/marked.min.js"></script>
  </head>
  <body>
    <div id="content"></div>

    <script type="text/babel">
      // To get started with this tutorial running your own code, simply remove
      // the script tag loading scripts/example.js and start writing code here.

      var CommentBox = React.createClass({
          render:function(){
            return (
                <div className="commentBox">
                  Hello,world! I am a CommentBox.
                 </div> 
              );
          }
        });

      ReactDOM.render(
          <CommentBox />,
          document.getElementById('content')
        );

    </script>
  </body>
</html>

```


注意，HTML 元素以小写开头，React 自定义 Class 以大写开头。

React 使用的 JSX 语法，它是原生 JavaScript 的语法糖，与下面的原生例子一样，但更简洁：

```javascript
var CommentBox = React.createClass({displayName: 'CommentBox',
  render: function() {
    return (
      React.createElement('div', {className: "commentBox"},
        "Hello, world! I am a CommentBox."
      )
    );
  }
});
ReactDOM.render(
  React.createElement(CommentBox, null),
  document.getElementById('content')
);
```

在这个例子中我们对一个 JavaScript 对象添加了一些方法，然后传递给 React.createClass() 创建新的组件。这些方法中最重要的是 `render`，它返回将要渲染成 HTML 的 React 组件。

`<div>` 标签不是实际的 Node 节点，它是 React 提供的 `div` 组件的实例。你可以吧这些当作标签或者 React 知道如何处理的数据。因此 React 是安全的，不会生成 HTML 字符串，所以可以避免受到 XSS 攻击。

请不要在 render 返回基础 HTML，你可以返回由你 (或其他人) 构建的组件树,这是 React 可组合的原因，前端维护的关键原则。

`ReactDOM.render()` 实例化根组建，启动框架，并注入标记到原生 DOM，给第二个参数。

`ReactDOM` 模块暴露指定的 DOM 操作方法，它是 React 在不同的平台的核心工具。

## 第二个例子

这个例子在上一个例子的基础上添加两个自定义 Class，`<CommentList />` 和 `<CommentForm />` ：

```javascript

	  var CommentList = React.createClass({
        render:function(){
          return (
              <div className="commentList">
                Hello,world!I am a CommentList.
                </div>
            );
        }
      });

      var CommentForm = React.createClass({
        render : function(){
          return (
              <div className="commentForm">
                Hello,world! I am a CommentForm.
               </div> 
            );
        }
      });


       var CommentBox = React.createClass({
          render:function(){
            return (
                <dic className="commentBox">
                  <h1>Comments</h1>
                  <CommentList />
                  <CommentForm />
                 </dic> 
              );
          }
        });

      ReactDOM.render(
          <CommentBox />,
          document.getElementById('content')
        );



    </script>
  </body>

```

我们将 HTML 标签的组件混合在一起构建。HTML 组件也是正常的 React 组件，就像你所定义的组件一样，有一点不同。JSX 编译器会自动使用 `React.createElement(tagName)` 重写 HTML 标签，这是为了防止污染全局命名空间。

## 使用 props

让我们来创建一个 comment，它依赖从上级传递过来的数据，从上级传递来的数据可以通过 `property` 使用，我们通过 `this.props` 访问 properties，通过 props，comment 可以读取从 CommentList 读取数据，和渲染一些标记。

```javascript

var Comment = React.createClass({
        render : function(){
            return (
                <div className="comment">
                  <h2 className="commentAuthor">
                    {this.props.author}
                  </h2>
                  {this.props.children}  
                </div>
              );  
          }
      });

```

在 JSX 语法的括号内 (属性或子级) 可以写 JavsScript 表达式，你可以放置文本或 React 组件到树中。我们通过 `this.props` 访问组件的属性名，`this.props。children` 访问组件的内嵌元素。

## 组件属性

我们已经定义了 `Comment` 组件，我们想传递作者名和评论文本给 `Comment` 组件。这样我们就可以使用一份代码完成所有的单独评论工作。现在我们开始添加：

```javascript

var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        <Comment author="Pete Hunt">This is one comment</Comment>
        <Comment author="Jordan Walke">This is *another* comment</Comment>
      </div>
    );
  }
});

```

现在我们已经从上级 `CommentList` 组件传递了一些数据给子级 `Comment` 组件。例如我们传递了 Pete Hunt (通过属性) 和它的评论文本 (通过类 XML 子级) 给第一个 `Comment`。然后如之前所说，`Comment` 组件将会通过 `this.props.author` 访问 properties，和通过 `this.props.children` 访问子级。

> `CommentList` 组件包含 `Comment` 组件，这里的 `Comment` 组件被传递了属性和子级。在 `Comment` 组件的定义中 (React.createClass()) 就可以通过 `this.props` 访问这些值。


## 添加 Markdown

Markdown 是一种简单的排版格式。

首先在页面引入第三方库 `<script src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.2/marked.min.js"></script>`。然后调用函数即可将字符串转换成 HTML。

接着，我们将评论使用 Markdown 转换成 HTML。

```javascript

var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {marked(this.props.children.toString())}
      </div>
    );
  }
});

```

为了正确的调用 Marked 库，我们需要将值使用 `.toString()` 转换成字符串。现在出现了一个问题，我们的评论成了 `<p>This is <em>another</em> comment</p>` 这样字符！我们想看到的是实际的 HTML。

这是 React 为了防止 **XSS 攻击**。下面可以绕过这个问题，但框架不建议你这样做！







