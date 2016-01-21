# Native 模块(Android)

有时候一个应用需要访问 React Native 平台目前没有对应模块的 API 。也许你需要复用一些已经存在的 Java 代码而不需要在 JavaScript 里面重新实现，或者写一些高性能，多线程的代码，比如图片处理，数据库，或者任何先进的扩展。

我们设计了 React Native 以致于你可以写一些真正的原生代码并且可以完全拥有系统的权限的能力。这是一个更加先进的特征，并且我们不希望这是传统开发过程中的一部分，然而它存在是非常重要的。如果 React Native 不支持你需要的原生特征，那么你应该可以自己创建它。

## Toast 模块

这个引导将会使用这个 [Toast](http://developer.android.com/reference/android/widget/Toast.html) 的例子。我们将会可以通过使用 JavaScript 创建一个 toast 消息。

我们从创建一个原生模块开始。一个原生模块是一个通常继承 `ReactContextBaseJavaModule` 类的  Java 类，并且实现了 JavaScript 需要实现的方法。我们这里的目标是允许通过使用 JavaScript 书写 
 `ToastAndroid.show('Awesome', ToastAndroid.SHORT);`就可以在屏幕上面显示一个短短的 toast 消息。

```java
package com.facebook.react.modules.toast;

import android.widget.Toast;

import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;

import java.util.Map;

public class ToastModule extends ReactContextBaseJavaModule {

  private static final String DURATION_SHORT_KEY = "SHORT";
  private static final String DURATION_LONG_KEY = "LONG";

  public ToastModule(ReactApplicationContext reactContext) {
    super(reactContext);
  }
}
```

`ReactContextBaseJavaModule`  需要一个叫做 `getName` 的方法被实现。这个方法的目的就是返回在 JavaScript 里面表示这个类的叫做 `NativeModule` 的字符串的名字。在这里我们调用 `ToastAndroid` 因此我们可以在 JavaScript 里面使用 `React.NativeModules.ToastAndroid` 来得到它。

```java
  @Override
  public String getName() {
    return "ToastAndroid";
  }
```

一个可选的叫做 `getConstants` 的方法会将传递给 JavaScript 的常量返回。这个方法的实现并不是必须的，但是却对在 JavaScript 和 Java 中同步的预定义的关键字的值非常重要。

```java
  @Override
  public Map<String, Object> getConstants() {
    final Map<String, Object> constants = new HashMap<>();
    constants.put(DURATION_SHORT_KEY, Toast.LENGTH_SHORT);
    constants.put(DURATION_LONG_KEY, Toast.LENGTH_LONG);
    return constants;
  }
```

给 JavaScript 暴露一个方法，一个 Java 方法需要使用 `@ReactMethod` 来注解。桥接的方法的返回值类型总是 `void`。React Native 的桥接是异步的，因此将一个结果传递给 JavaScript 的唯一方式就是使用回调函数或者调用事件（见下面）。

```java
  @ReactMethod
  public void show(String message, int duration) {
    Toast.makeText(getReactApplicationContext(), message, duration).show();
  }
```

### 参数类型

下面的参数类型是被使用 `@ReactMethod` 注解的方法支持的，并且它们直接对应 JavaScript 中对应的值。

```
Boolean -> Bool
Integer -> Number
Double -> Number
Float -> Number
String -> String
Callback -> function
ReadableMap -> Object
ReadableArray -> Array
```

### 注册模块

在使用 Java 的最后一步就是注册这个模块，这将在你的应用包中的 `createNativeModules` 发生。如果一个模块没有被注册，那么它在 JavaScript 是不可用的。

```java
class AnExampleReactPackage implements ReactPackage {

  ...

  @Override
  public List<NativeModule> createNativeModules(
                              ReactApplicationContext reactContext) {
    List<NativeModule> modules = new ArrayList<>();

    modules.add(new ToastModule(reactContext));

    return modules;
  }
```

当包被创建的时候，它需要提供给 ReactInstanceManager 。可以看  `UIExplorerActivity.java` 这个例子。当你初始化一个新工程的时候默认的包是`MainReactPackage.java`。

```java
mReactInstanceManager = ReactInstanceManager.builder()
  .setApplication(getApplication())
  .setBundleAssetName("AnExampleApp.android.bundle")
  .setJSMainModuleName("Examples/AnExampleApp/AnExampleApp.android")
  .addPackage(new AnExampleReactPackage())
  .setUseDeveloperSupport(true)
  .setInitialLifecycleState(LifecycleState.RESUMED)
  .build();
```

为了能让你更加方便的从 JavaScript 访问你的新功能的时候，通常会将原生模块包裹在一个 JavaScript 模块里面。这不是必须的，但是节省了你的类库的使用者每次都要 pull  `NativeModules` 的不便。这个 JavaScript 文件也为你增加任何 JavaScript 端功能提供了方便。

```java
/**
 * @providesModule ToastAndroid
 */

'use strict';

/**
 * This exposes the native ToastAndroid module as a JS module. This has a function 'showText'
 * which takes the following parameters:
 *
 * 1. String message: A string with the text to toast
 * 2. int duration: The duration of the toast. May be ToastAndroid.SHORT or ToastAndroid.LONG
 */
var { NativeModules } = require('react-native');
module.exports = NativeModules.ToastAndroid;
```

现在，在你的 JavaScript 文件里面你可以像下面这样调用方法：

```js
var ToastAndroid = require('ToastAndroid')
ToastAndroid.show('Awesome', ToastAndroid.SHORT);

// Note: We require ToastAndroid without any relative filepath because
// of the @providesModule directive. Using @providesModule is optional.
```

## 远不止 Toasts

### 回调

原生模块也提供了一种特殊的参数－一个回调。在大多数情况下这是给 JavaScript 返回结果使用的。

```java
public class UIManagerModule extends ReactContextBaseJavaModule {

...

  @ReactMethod
  public void measureLayout(
      int tag,
      int ancestorTag,
      Callback errorCallback,
      Callback successCallback) {
    try {
      measureLayout(tag, ancestorTag, mMeasureBuffer);
      float relativeX = PixelUtil.toDIPFromPixel(mMeasureBuffer[0]);
      float relativeY = PixelUtil.toDIPFromPixel(mMeasureBuffer[1]);
      float width = PixelUtil.toDIPFromPixel(mMeasureBuffer[2]);
      float height = PixelUtil.toDIPFromPixel(mMeasureBuffer[3]);
      successCallback.invoke(relativeX, relativeY, width, height);
    } catch (IllegalViewOperationException e) {
      errorCallback.invoke(e.getMessage());
    }
  }

...
```

使用以下方法可以来访问在 JavaScript 里面可以使用：

```js
UIManager.measureLayout(
  100,
  100,
  (msg) => {
    console.log(msg);
  },
  (x, y, width, height) => {
    console.log(x + ':' + y + ':' + width + ':' + height);
  }
);
```

一个原生模块支持只调用一次它的回调。它可以保存这个回调，并且在以后调用。

有一点需要强调的就是在原生方法完成之后这个回调并不是立即被调用的－请记住桥接通信是异步的，因此这个也在运行时循环里面。

## 线程

原生模块不应该设想有它们将在哪些线程里面被调用，因为目前的任务在以后改变是主要的。如果一个块调用是必须的，那么耗时操作将会被分配到间歇性的工作线程中，并且任何回调将会从这里开始。

## 给 JavaScript 传递事件

原生模块可以不需要立即被调用就可以给 JavaScript 发送事件。最简单的方式就是使用从 `ReactContext` 获得的 `RCTDeviceEventEmitter`，就像下面的代码片段：

```java
...
private void sendEvent(ReactContext reactContext,
                       String eventName,
                       @Nullable WritableMap params) {
  reactContext
      .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
      .emit(eventName, params);
}
...
WritableMap params = Arguments.createMap();
...
sendEvent(reactContext, "keyboardWillShow", params);
```

JavaScript 模块在那时可以通过使用 `Subscribable` 的 `addListenerOn` 来注册并且接收事件。

```js
var RCTDeviceEventEmitter = require('RCTDeviceEventEmitter');
...

var ScrollResponderMixin = {
  mixins: [Subscribable.Mixin],


  componentWillMount: function() {
    ...
    this.addListenerOn(RCTDeviceEventEmitter,
                       'keyboardWillShow',
                       this.scrollResponderKeyboardWillShow);
    ...
  },
  scrollResponderKeyboardWillShow:function(e: Event) {
    this.keyboardWillOpenTo = e;
    this.props.onKeyboardWillShow && this.props.onKeyboardWillShow(e);
  },
```
