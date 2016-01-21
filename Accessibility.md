# 辅助功能

## iOS

在 iOS 系统上辅助功能涵盖许多话题，但对许多人来说辅助功能是 VoiceOver 的代名词，即 iOS 3.0 版本以后的一种技术。它充当屏幕阅读器的角色，允许有视觉障碍的人使用 iOS 设备。点击[这里](https://developer.apple.com/accessibility/ios/)了解更多。

## Android 

对 Android 系统而言，辅助功能涉及到了许多不同的话题，其中之一是让丧失视力的人能够使用您的应用程序。对于现在的社会，谷歌提供了一个名叫 TalkBack 的内置屏幕读者服务机器人。使用该机器人，你可以使用触摸勘探和手势来使用移动设备和应用程序。TalkBack 可以使用文本语音转换器来阅读屏幕上的内容并且可以发出警报来通知用户有关于应用程序中的重要信息。点击[这里](https://support.google.com/accessibility/android)来了解更多关于 Android 的辅助功能的特征以及点击[这里](https://developer.android.com/guide/topics/ui/accessibility)来了解更多关于使您的本地应用程序的辅助功能。

## 创建辅助性应用程序

### 辅助功能的性质

#### 辅助性（iOS, Android）

如果为 `true`的情况，代表该视图是一个辅助功能元素。当视图是辅助功能元素时，它把它的子元素分组成一个单一的可选组件。默认情况下，可触摸的所有元素都具有辅助性。

在 Android 系统中，在 react-native  视图中 ' accessible={true}' 属性将被翻译成本地命令 ' focusable={true}'。

```java
<View accessible={true}>
  <Text>text one</Text>
  <Text >text two</Text>
</View>
```

在上面的示例中，我们不能分别在 'text one' 和 'text two' 中获得辅助焦点。相反我们可以在父元素上使用 'accessible'  属性获得焦点。

#### accessibilityLabel (iOS, Android)

如果要将视图标记为具有辅助性，那么一个比较好的做法就是为这个视图设置一个 accessibilityLabel 标签以便使用 VoiceOver 的人知道他们选择了什么元素。当用户选择了一些元素，那么 VoiceOver 将会阅读响应的字符串文本。

若要使用它，在您的视图中将 `accessibilityLabel` 属性设置为一个自定义的字符串：

```java
<TouchableOpacity accessible={true} accessibilityLabel={'Tap me!'} onPress={this._onPress}>
  <View style={styles.button}>
    <Text style={styles.buttonText}>Press me!</Text>
  </View>
</TouchableOpacity>
```
在上面的示例中，TouchableOpacity 元素中的 `accessibilityLabel` 会被默认的设置为 "点击我！"。 该标签是通过使用空格符来串联所有文本节点子元素构造的。

#### accessibilityTraits (iOS) 

辅助功能特征告诉人们他们在使用 VoiceOver 的时候选择了什么元素。此元素是一个标签？一个按钮？还是标头？ `accessibilityTraits` 将会回答这些问题。

如果要使用它，请把 accessibilityTraits 属性设置为 accessibilityTraits 辅助功能字符串(或数组)之一：

- **none** 当元素没有特征的时候使用。
- **button** 当元素需要被当做一个按钮的时候使用。
- **link** 当元素需要被当做链接的时候使用。
- **header** 当元素作为内容部分的标题 (如导航栏中的标题) 的时候使用。
- **search** 当文本字段元素也被视为一个搜索字段的时候使用。
- **image** 当元素需要被作为图像，比如和按钮和链接结合的时候使用。
- **selected** 当该元素被选中时使用。例如，表中被选中的行或者分段控件中选中的按钮。
- **plays** 当元素被激活的并且播放自己的声音的时候使用。
- **key** 当元素充当键盘按键的时候使用。
- **text** 当元素应该被视为不能更改的静态文本的时候使用。
- **summary** 当在应用程序首次启动的时候，该元素可以提供应用程序的实时状况的摘要的时候使用。例如，当关于天气的应用程序首次启动的时候，带有当天天气信息的元素将被该特征所标记。
- **disabled** 当控件未启动并且对用户的输入无响应的时候使用。
- **frequentUpdates** 当元素经常更新其标签或者它的值，但是太平凡的发送通知的时候使用。允许辅助功能客户端轮询更改。秒表就是一个例子。
- **startsMedia** 当激活一个元素并开始一段媒体会话(例如播放电影，录制音频)不应该被辅助技术的输出所打断，比如 VoiceOver。
- **adjustable** 当元素可以被"调整"的时候使用(例如滑块)。
- **allowsDirectInteraction** 当元素允许 VoiceOver 用户直接进行触摸互动的时候使用(例如，表示一个钢琴键盘的视图)。
- **pageTurn** 当它完成阅读的元素的内容时候通知 VoiceOver 需要滚动到下一个页面。

#### onAccessibilityTap (iOS)

使用此属性来分配一个自定义的函数，当有人在一个可访问元素被选中的时候通过双击来激活它的时候来调用该函数。

#### onMagicTap (iOS) 

当有人使用 “magic tap”手势，即：用两个手指双击的时候，该属性就会被分配给一个自定义函数，同时，这个函数会被调用。一个魔法敲击函数应该执行用户可以在组件中找到的最具有相关性的操作。在 iPhone 的手机应用程序中，一个魔法敲击可以接听或者结束一个电话。如果所选的元素不具有 `onMagicTap` 功能，该系统将遍历视图层次结构直到它找到一个拥有此功能的视图。

#### accessibilityComponentType (Android) 

在某些情况下，我们也要提醒选定的组件类型的最终用户 (即，它是一个"按钮")。如果我们正在使用本机的按钮，那么它会自动工作。由于我们使用的是 javascript，所以我们需要为  TalkBack 提供更多的语境。为了达到这个目的，您必须为所有 UI 组件指定 'accessibilityComponentType' 属性。例如，我们支持 'button'，'radiobutton_checked' 和 'radiobutton_unchecked'等。

```java
<TouchableWithoutFeedback accessibilityComponentType=”button”
  onPress={this._onPress}>
  <View style={styles.button}>
    <Text style={styles.buttonText}>Press me!</Text>
  </View>
</TouchableWithoutFeedback>
```

在上面的示例中，TouchableWithoutFeedback 是被 TalkBack 作为一个本机按钮声明的。

#### accessibilityLiveRegion (Android)

当组件动态的更改时，我们希望 TalkBack 去提醒最终用户。'AccessibilityLiveRegion' 属性让这成为可能。它可以被设置为 ‘none’, ‘polite’ 以及 ‘assertive’。

- **none** 辅助功能服务不应该对此视图通知改变的地方。
- **polite** 辅助功能服务应该对此视图通知改变的地方。
- **assertive** 辅助功能服务应该中断正在进行的会话，并且以立即宣布该视图的改变。

```java
<TouchableWithoutFeedback onPress={this._addOne}>
  <View style={styles.embedded}>
    <Text>Click me</Text>
  </View>
</TouchableWithoutFeedback>
<Text accessibilityLiveRegion="polite">
  Clicked {this.state.count} times
</Text>
```

在上面的示例方法 _addOne 更改了 state.count 变量。当最终用户单击 TouchableWithoutFeedback 的时候，因为 TalkBack 的  'accessibilityLiveRegion=”polite”' 属性，所以它读取了文本视图中的文本。

#### importantForAccessibility (Android)

对于两个重叠的并且拥有相同父元素的 UI 组件，默认的辅助功能焦点可以有不可预知的行为。如果一个视图触发辅助事件并且它被汇报给了辅助功能服务器，那么 'ImportantForAccessibility'  属性将会通过控制解决它，它可以被设置为‘auto’, ‘yes’, ‘no’ 以及 ‘no-hide-descendants’ (最后一个值将迫使辅助功能服务忽略该组件和它的所有子元素)。

```java
<View style={styles.container}>
  <View style={{position: 'absolute', left: 10, top: 10, right: 10, height: 100,
    backgroundColor: 'green'}} importantForAccessibility=”yes”>
    <Text> First layout </Text>
  </View>
  <View style={{position: 'absolute', left: 10, top: 10, right: 10, height: 100,
    backgroundColor: 'yellow'}} importantForAccessibility=”no-hide-descendant”>
    <Text> Second layout </Text>
  </View>
</View>
```

在上面的示例中，对于 TalkBack 以及其他的辅助功能服务而言，黄色的布局及其后代是完全不可见的。所以我们可以容易的使用来自于同一个父元素并且不带有令人疑惑的 TalkBack 的视图。

### 发送辅助功能事件（Android）

有时候在 UI 组件中去触发一个辅助功能事件很有用 (即当一个自定义的视图在屏幕上显示或自定义单选按钮已被选中)。为了达到这个目的，本地 UIManager 模块公布了一个名叫 'sendAccessibilityEvent' 的方法。它拥有两个参数： 视图标签和一个类型的事件。

```java
_onPress: function() {
  this.state.radioButton = this.state.radioButton === “radiobutton_checked” ?
  “radiobutton_unchecked” : “radiobutton_checked”;
  if (this.state.radioButton === “radiobutton_checked”) {
    RCTUIManager.sendAccessibilityEvent(
      React.findNodeHandle(this),
      RCTUIManager.AccessibilityEventTypes.typeViewClicked);
  }
}

<CustomRadioButton
  accessibleComponentType={this.state.radioButton}
  onPress={this._onPress}/>
```

在上面的例子中，我们创建了一个如同本按钮的自定义单选按钮。更具体地说，TalkBack 可以正确的公布单选按钮选择的变化。

## 测试 VoiceOver 支持的内容（iOS）

如果要启用 VoiceOver，那么请在你的 iOS 设备上打开设置应用程序的位置。点击 General，然后点击 Accessibility。那里你会发现许多人们用来优化他们的设备的工具，比如粗体文本、 增加的对比度以及 VoiceOver。

如果要启用 VoiceOver，点击  "Vision" 下的 VoiceOver，打开显示在顶部的开关。

在辅助功能设置的最底部，还有一个"辅助功能的快捷方式"。你可以使用它三次单击主页按钮来触发 VoiceOver。
