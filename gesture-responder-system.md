# 手势应答系统

手势识别在移动设备上比在网络上要复杂得多。当应用程序确定用户的意图时，一个触摸可能要经历几个阶段。例如，应用程序需要确定触摸是否是滚动，滑动部件还是轻击。这甚至可以在触摸期间发生改变，也可以有多个同时触摸。

要想使组件在没有任何额外的关于它们的父组件或子组件的知识的情况下处理这些触摸交互，需要触摸应答系统。这个系统在 `ResponderEventPlugin.js` 中实现了，其中包含更多细节和文档。

### 最佳实践

用户在 web 应用程序与本机的可用性上可以感觉到巨大的差异，并且这是最大的原因之一。每一个动作都应该有以下属性：

- 反馈/高亮——显示给用户是什么正在处理他们的触摸，以及当他们释放手势时，会发生什么
- 撤销的能力——当做一个动作时，用户应该能够在触摸过程中通过移动手指中止该动作。

这些特性让用户使用一个应用程序时更舒适，因为它允许人们在实验和交互时不用担心犯错误。

### TouchableHighlight 和 Touchable* 

应答系统在使用时可能是复杂的。所以我们为应该“可以轻击的”东西提供了一个抽象的 `Touchable` 实现。这使用了应答系统，并且使你以声明的方式可以轻松地识别轻击交互。在网络中任何你会用到按钮或链接的地方使用 `TouchableHighlight`。

## 应答器生命周期

通过实施正确的处理方法，视图可以成为接触应答器。有两种方法来询问视图是否想成为应答器：

- `View.props.onStartShouldSetResponder: (evt) => true,`——这个视图是否在触摸开始时想成为应答器？
- `View.props.onMoveShouldSetResponder: (evt) => true,`——当视图不是应答器时，该指令被在视图上移动的触摸调用：这个视图想“声明”触摸响应吗? 

如果视图返回 true 并且想成为应答器，那么下述的一种情况就会发生：

- `View.props.onResponderGrant:(evt)= > { }` ——视图现在正在响应触摸事件。这个时候要高亮标明并显示给用户正在发生的事情。
- `View.props.onResponderReject:(evt)= > { }`——其他的东西时应答器并且不会释放它。

如果视图正在响应，那么可以调用以下处理程序：

- `View.props.onResponderMove:(evt)= > { }`——用户正移动他们的手指
- `View.props.onResponderRelease:(evt)= > { } `——在触摸最后被引发，即“touchUp”
- `View.props.onResponderTerminationRequest:(evt)= >true`——其他的东西想成为应答器。这种视图应该释放应答吗？返回 true 就是允许释放
- `View.props.onResponderTerminate:(evt)= > { } `——应答器已经从视图获取了。可能在调用 `onResponderTerminationRequest` 之后被其他视图获取，也可能是被操作系统在没有请求的情况下获取了(发生在 iOS 的 control center/notification center)

`evt` 是一个综合的触摸事件，有以下形式：

- `nativeEvent`

	- `changedTouches`——自从上个事件之后，所有发生改变的触摸事件的数组
	- `identifier`——触摸的 ID
	- `locationX` ——触摸相对于元素的 X 位置
	- `locationY`——触摸相对于元素的 Y 位置
	- `pageX`——触摸相对于屏幕的 X 位置
	- `pageY`——触摸相对于屏幕的 Y 位置
	- `target `——接收触摸事件的元素的节点 id
	- `timestamp`——触摸的时间标识符，用于速度计算
	- `touches`——所有当前在屏幕上触摸的数组

### 捕捉 ShouldSet 处理程序

在冒泡模式，即最深的节点最先被调用，的情况下，`onStartShouldSetResponder` 和 `onMoveShouldSetResponder` 被调用。这意味着，当多个视图为 `* ShouldSetResponder` 处理程序返回 true 时，最深的组件会成为应答器。在大多数情况下，这是可取的，因为它确保了所有控件和按钮是可用的。

然而，有时父组件会想要确保它成为应答器。这可以通过使用捕获阶段进行处理。在应答系统从最深的组件冒泡时，它将进行一个捕获阶段，引发 `* ShouldSetResponderCapture`。所以如果一个父视图要防止子视图在触摸开始时成为应答器，它应该有一个 `onStartShouldSetResponderCapture` 处理程序，返回 true。

- `View.props.onStartShouldSetResponderCapture: (evt) => true,`
- `View.props.onMoveShouldSetResponderCapture: (evt) => true,`

### PanResponder

更高级的手势解释，看看 [PanResponder](pan-responder.md)。
