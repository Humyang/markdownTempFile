# ReactiveCocoa 笔记

使用的版本是 4.0.2-alpha-1，支持 Xcode 7 和 Swift 2，相比较 3.x 版本的使用方式有一些变化，例如去掉了 `|>`，改为直接使用 `.` 方式调用，其它改变还在探索中。

## 安装过程

官方推荐的方式是将 ReactiveCocoa 作为 sudmodule 添加到项目中，但是我老是编译不通过，所以只能使用 CocoaPods 方法。

在 CocoaPods 的 Podfile 添加：

```swift
use_frameworks!
pod 'ReactiveCocoa', '4.0.2-alpha-1'
```

> 因为 ReactiveCocoa 是使用 Swift 的框架，所以必须添加 use_frameworks!，否则回报错：
> [!] Pods written in Swift can only be integrated as frameworks; add `use_frameworks!` to your Podfile or target to opt into using it. 

然后执行 pod install ，等待框架下载。

下载完成后，需要在项目的 TARGETS 的 General 标签中的 Linked Frameworks and Libraries 项添加 ReactiveCocoa.framework 和依赖的 Result.framework，如图：

![](./1-1.png)

使用时别忘了在文件顶部导入框架 `import ReactiveCocoa`。

## Signal 部分源代码分析

官网上给出的基本使用方法

```swift
        let (signal, sink) = Signal<String, NoError>.pipe()
        
        signal.map { str in
            return str.uppercaseString
            }.observe { eve in print(eve.value!) }

        
        sendNext(sink, "a")     // Prints A
        sendNext(sink, "b")     // Prints B
        sendNext(sink, "c")     // Prints C
```

Pipe 是一个可以手动控制的 signal，通过 `Signal.pipe()` 创建，这个方法会返回 signal 和 observer，可以通过发送 event 到 observer ,event 会到达 signal，经过 map，fifter 等方法，最后到达 observe。

例如，可以替代在登陆处理时使写的 block 回调方法，block 只需要简单的发送 event 给 observer，这样就可以隐藏回调方法的实现细节，也方便测试。

上面的例子 event 发送到 observe 之前都需要先经过 map 的处理。类似的有 fifter，只有满足条件才向下传递 event。

查看 map 的实现代码比较麻烦，不能通过 option + click 直接跳转，只能在框架的源文件中查找，map 位于 Signal.swift 中：


```swift
public func map<U>(transform: T -> U) -> Signal<U, E> {
		return Signal { observer in
			return self.observe { event in
				observer(event.map(transform))
			}
		}
	}
```

因为 map 需要的参数只有一个，并且是 function 类型，所以符合尾式 Closures 的要求：

```swift
signal.map { str in
            return str.uppercaseString
            }
            
//等价于            

signal.map({
str in
return str.uppercaseString
}) 

//更详细一点

signal.map ({ (let str:String) in
            return str.uppercaseString
            })
            
```

map 源代码中 `transform: T -> U` 的 T 在源代码中定义为 `public final class Signal<T, E: ErrorType>`，根据我们的声明 `let (signal, sink) = Signal<String, NoError>.pipe()`，所以 T 的类型是 `String`。

而 U 由 map 的返回类型决定，最终 map 会返回一个 Signal<U, E>,E 则是定义 signal 时指定的错误类型，我们这里指定的是 NoError， `let (signal, sink) = Signal<String, NoError>.pipe()`。


当为 signal 绑定多个 ovserve 时，Event 会同时发送给所有 observe：

```swift
        signal.map { str in
            return str.uppercaseString
            }.observe { eve in print(eve.value!) }

        signal.map ({ (let str:String) in
            return str.uppercaseString
            }).observe { eve in print(eve.value!) }
        
        sendNext(sink, "a")     // Prints A
        sendNext(sink, "b")     // Prints B
        sendNext(sink, "c")     // Prints C
```


## 一些想象中的使用例子

