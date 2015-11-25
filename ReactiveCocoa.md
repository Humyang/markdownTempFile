# ReactiveCocoa

## 了解 ReactiveCocoa

### Event 约定

Event 是 ReactiveCocoa 的基础，Signals 和 Signal producers 都会发送 Event，它们可以统一被称为 event streams。

event streams 必须遵循下面的语法：

`Next* (Interrupted | Error | Completed)?`

这说明了一个 event steams 包含：

1. 任意数量的 `Next` event。
2. 可以选择通过一个 event 来中断 event stream，这个 event 可以是 `Interrupted`, `Error`, 或 `Completed`。

发送中断 event 之后，event stream 不会再接收任何 event

#### `Next` 附加值或标识到所产生的 event 中

Next Event 所负载的内容称为 “值”。只有 Next Event 才可以附加值。一个 event stream 可以包含任意数量的 `Next`，所以对于值的使用有一些约束，包括它们的类型必须一致。  

例如，值可能代表集合中的一个元素，或用来处理一些长时间运行的计算，甚至 Next event 的值什么都不代表，通常这种情况值的类型是 `()`，表明发生了一些事情，但不详细说明发生了什么事情。

对于 event stream 的操作大部分建立于 Next event 之上，它代表拥有 “有意义的数据” 的 signal 或 producer。

#### Error 的行为类似发生异常，它会立刻发送

`Error` event 表明发生了一些错误，并会指出具体错误发生的原因。错误是致命的，因此会尽可能的快速传递给 event stream 处理。

Error 也表现得像异常一样，它会跳过所有操作，中断 event stream。换句话说，接收到错误时大部分操作会立刻停止，然后往前传播错误。

所以，`Error` eveny 应该用在中断异常上，如果一个重要的运算完成了它的工作，那么使用 `Next` 描述结果会更加合适。

如果你确定 event stream 永远不会发生错误，那么可以把参数指定为 `NoError` 类型，它使用静态的方式保证 `Error` event 不会发送到 event stream 中。

#### Completion 标识着成功

当运算已经完成时 event stream 可以发送 `Competed` event，或者指示该 stream 已经正常中断。

许多运算会操作 `Completed` event 使 event stream 的生命周期缩短或延长。例如，`take` 将会在接收到特定值的数字后完成运算，从而更早中断 event stream。另一方面，大部分运算会接收多个 signal 或 producers ，因此转发 `Completed` 之前会等待直到它们所有都完成，因为该 event stream 依赖所有的 signal 的输入。

#### Interruption 取消所有外部的工作并且通常立即传播。

当 event stream 应该取消处理时发送 `Interruption` event。Interruption 处于成功和错误之间：运算还未成功，因为它没有完成，但它不需要进行错误处理。

大部分的运算会立即中断传播，但有少数例外。例如，`flattening` 运算会忽略它 `inner` producers 所发生的 `Ibterrupted` event，来自 inner 的取消应该不需要取消更大的工作单位。

RAC 将自动发送 `Interrupted` event 到 [disposal](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/Documentation/FrameworkOverview.md#disposables) 之上，如果需要也可以手动发送。除此之外，[custom operators](https://github.com/ReactiveCocoa/ReactiveCocoa/blob/master/Documentation/DesignGuidelines.md#implementing-new-operators) 必须确保 Interruption event 会转发到观察者。

#### Events 是串行接收的

RAC 会保证 Event 的抵达顺序。换句话说，signal 或 producer 不可能同时接收多个 Event，即使 event 在多个线程中同时发送。

这简化了 [operator]() 的实现和  observers。

#### Event 不能递归发送

与 RAC 确保 event 将不会同时接收一样，RAC 也确保它们不会被递归接收。所以 operators 和 observers 不需要折返操作。

如果一个 event 从线程发送到 signal 中但该事件之前已经被 signal 处理，那么会产生死锁。这是因为递归 signal 通常会使程序员产生错误，确定死锁比不确定的死锁难判断。

当明确需要使用递归 signal 时，递归 event 应该添加时间偏移，例如 `delay` 运算，确保之前发送的 event 处理完成之前它不会发送。

#### Event 默认情况下同步发送

RAC 没有明确说明是并发还是异步，可以使用 scheduler 调度 Operators，但它必须明确通过框架的 consumer 调用。

“vanilla” signal 或 producer 默认情况下将同步发送它的所有 event，意味着所有它所需要发送的 event 将会同步调用 observer，底层的工作在 event 处理完成之前将不会恢复。 

类似 NSNotificationCenter 或 UIControl ，event 是分布式的。

### Signal

signal 是一个总是打开的，遵循 event 约定的 stream。

`Signal` 是引用类型，因为每个 signal 都有一个标识，换句话说，每个 signal 都有它自己的生命周期，一旦中断 signal 后，将无法重新启用。

#### Signal 在实例化后开始工作

`Signal.init` 会将传递给它的 generator closure 立即执行。这有一个 Side Effects:初始化返回之前可能会有事件发送的。

对于在初始化返回之前发送的事件，是不可能被接收到的。

#### 观测一个 signal 不会有 side effects

与 `Signal` 关联的工作不会因为 [observers]() 的添加或移除而停止，因此 `observe` 方法 (或移除它们的方法) 永远不会发生 side effects。

signal 的 side effects 只能通过中断 event （`Interrupted`, `Error`, 或 `Completed`）终止。

#### signal 的所有观测者以同样的顺序见到相同的 event

因为对 signal 的观测不会造成 side efects，所以 `Signal` 永远不会为不同的观察者定制不同的 event。当一个 event 发送到 signal 上时，它将会同步的分配到所有观测者中，非常像 `NSNotificationCenter` 发送通知。

换句话说，每个观测者的 “event 时间线” 都是一样的。所有的观测者都会见到同样的 event stream。

这里有一个例外的规则：在 event 已经中断后所添加的观测者会接收到一个 `Interrupted` event。

#### signal 会一直保留直到底层的观测者被释放

即便调用者没有维持对 `Signal` 的引用：

* 由 `Signal.init` 创建的 signal 会一直保持存活状态直到 generator closure 释放 [observer]() 参数
* 由 Signal.pipe 创建的 signal 会保存存活状态知道它返回的 observer 被释放。

这确保了关联的 signal 将会长时间运行工作不会过早释放。

记住 signal 有可能会在中断 event 发送之前被释放。应该尽量避免这种情况，这会导致资源泄露，但有时禁用终止处理是有用的。

#### 使用中断 event 处理 signal 资源

当中断 event 发送到 `Signal` 时，所有 [observers]() 将会被释放，任何用作产生 event 的资源都应该进行处理。

这里有一个最简单的方法确保资源正确清理，时通过 generator closure 返回的 [disposable]()，它将会在产生中断 event 时进行处理。disposable 应该负责释放内存，关闭文件距柄，取消网络请求，或任意其他可能已经执行过的工作。


### SignalProducer 约定

[signal producer]() 就像一个 “食谱” 创建 [signal]() ，Signal producers 自己并不会做任何事情，只有当 signal 是 produced 是才开始工作。

signal oriducer 只是声明如何创建 signal，它是值类型，对于它来说没有内存管理可言。

#### Signal producer 开始工作需要通过创建 signal

start 和 startWithSignal 方法 (分别是隐式的和显式的) 都会生产 `Signal`。signal 实例化之后，传递到 `SignalProducer.init` 的 closure 将会执行，任何 observer 绑定之后都会开始执行该 event 流。

尽管 producer 它自己实际不负责任何工作的执行，但它通常对 producer 通知 “starting” 和 “canceling”。这些术语指向的 signal 将会开始工作，并在停止工作时对 signal 进行处置。

producer 可以由任何数字开始 (包含 0)，与它关联的工作将会准确的执行。

#### 每个 produced signal 可以在不同的时间发送不同的 event

因为 signal producers 是根据需求开始工作，它可能有不同的 observers 关联到它的每个执行环境中，而这些 observers 可能会在不同的 event 时间线中完成。

换句话说，event 是从无到有产生的。。。。

signal producer 的每个执行环境都遵循 Event 约定。

#### Signal 操作可以从 signal producers 中解除

由于 signal 和 signal producers 之间的关系，它有可能自动的把运算应用到相同数字的 SignalProducer 中的一个或多个 Signal 中，可以使用 `lift` 方法。

lift 方法将会应用该特定操作的行为到每个 Signal 中，。。。。。。。。

#### Disposing 时 produced signal 将会被打断

当 producer 使用 `start` 或 `startWithSignal` 方法开始工作时，`Dispoable` 会自动创建并且会穿。

对象在 Disposing 时将会中断 produced 的 Signal，从而取消未完成的工作并发送一个 Interrupted Event 到所有观测者中，并在 SignalProducer.init 处置所有添加到 CompositeDisposable 中的东西。

记住 priduced `Signal` 的 disposing 将不会影响到其它由同一个 SignalProducer 创建的 signal。

### Best practics

下面是一些帮助你保持基于 RAC 的代码的可预测性，易懂性，和高性能的建议。

它们只是指引，还需要由你自己决定如何应用到你的代码中。

#### 在众多值中只处理需要的值

保持 event stream 的长时间存活需要耗费 CPU 和内存，不需要的工作所执行的结果是永远不会用到的。

如果只有某些值的数字或某些事件的数字需要从 signal 或 producer 获取，那么可以使用 take 或 takUntil 当某些条件达成时自动使 event stream 结束。

这样做的好处是明显的，因为这样可以更早的结束 signal，大量减少工作中的数量。

#### 在已知的调度中观测 event

当从未知位置的代码接收 signal 或 producer 时，它很难弄清楚哪个线程事件将会到达，尽管 event 保证是串行的，某些时候需要更强的保证，例如执行 UI 更新 (它必须发生在主线程)

这样的保证是很重要的，observeOn 运算符应该被用作强制使 event 被特定的 scheduler 接收。

#### 尽可能的少交换 schedulers

尽管上面说明 event 在绝对需要时应该只交个特定的 scheduler。交换 schedulers 会引入不必要的延迟和导致 CPU 负载提升。

通常，observeOn 应该只在观测 signal ，开始 producer，或绑定到 property 之前被使用。这样确保 event 会到达期望中的 scheduler，在它们到达之前不会在多线程之间跳跃。

#### 在 signal producers 内捕捉异常反应

因为 signal producers 是根据需求开始工作的，任何返回 signal producer 的 function 或 method 应该确保异常反应在 producer 内部被捕获，而不是成为调用 function 或 method 的一部分。

例如这个 function：

```swift
func search(text: String) -> SignalProducer<Result, NetworkError>
```

不应该马上开始搜索。

返回的 producer 应该在每次启动时执行一次搜索。这也意味着如果 producer 永远不启动，搜索永远不会被执行

#### 通过共享一个 produced signal 实现 signal producer 的异常反应的共享

如果多个观测者对 signal producer 的结果感兴趣，为每个观测者调用 `start` 意味着与工作关联的 producer 将会耗费大量时间执行并且结果可能不一致。

如果：

1. 观测者需要接收准确相同的结果
2. 观测者需要知道彼此
3. producer 的启动代码需要知道每一个观测者

可能由 producer 一次启动更适合，可以把一个 signal 的结果共享给所有观测者，(结果？是说 signal 的完成和未完成状态吗？)通过把它们附加到 closure 然后传递给 `startWithSignal` 方法。

#### 明确的 disposal 是最好的管理 operator 声明周期