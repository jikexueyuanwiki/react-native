# 已知 Issues

## 弃用的模块和原生视图   

这是 React Native Android 的首次发行，因此在 iOS 平台上出现的视图不一定都会发布在 Android 上面。我们对社区里面的对下一系列模块和视图的开源代码的反馈非常感兴趣。并不是所有的视图在 iOS 和 Android 上面都有 100% 完全相同的表现，因此在这里就有必要使用一个对应的例子：在 Android 上面使用 ProgressBar，而在 iOS 上面则会使用 ActivityIndicator 来替代。

我们对共同的视图和模块的临时计划包括：

视图

```
    View Pager
    Swipe Refresh
    Spinner
    ART
    Maps
    Webview
```
模块

```
    Geo Location
    Net Info
    Camera Roll
    App State
    Dialog
    Intent
    Media
    Pasteboard
    Alert
```
## 在 Android 上发布的模块   

现在在 Android 上面发布自定义的原生模块并不是一件轻松的事。贡献者平滑稳定的工作流很重要并且这将在第一次开源版本发布之后被密切关注。当然，我们的目标是形成流水线并且尽量优化在 iOS 和 Android 平台之间的流程。   

## 使用透明度为 0 来覆视图不能被点击   

在 iOS 和 Android 之间使用透明度为 0 来处理视图有一个明显的差异。虽然在 iOS 上面允许这些视图被点击并且在这些视图下面的视图也将会接收到触摸输入，但是在 Android 上面这个触摸输入则会被阻塞。这一点可以用下面的一个只能在 iOS 上面点击的例子来演示。

```
    <View style={{flex: 1}}>
        <TouchableOpacity onPress={() => alert('hi!')}>
            <Text>HELLO!</Text>
        </TouchableOpacity>
        <View style={{
          position: 'absolute', 
          top: 0, 
          left: 0, 
          bottom: 0, 
          right: 0, 
          opacity: 0}} />
    </View>
```

## 在 Android 上面的单独布局节点   

在 React Native 的安卓版本一个优化的特征就是对于只能贡献布局的视图不能有原生视图，只有它们的布局属性能够传递到它们的子视图。这个优化是为了给更深层次的 React Native 视图层提供支持，因此默认是开启的。如果你需要一个视图被呈现，或者间歇性的测试检测一个视图是不是只是布局，关闭这个行为就很有必要了。为了做到这一点，你只需要设置 `collapsable` 为 false 即可：

```
    <View collapsable={false}>
        ...
    </View>
```
