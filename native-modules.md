# Native 模块(iOS)

有时一个应用程序需要访问平台 API，React Native 并没有相应的封装器。也许你想重用现有的一些 Objective——C 或 C++ 代码，无需在 JavaScript 上重新实现。或者写一些高性能，多线程的代码，如图像处理、网络堆栈，数据库或渲染。

我们设计 React Native，这样可以为你写真正的本地代码，并且能够访问整个平台。这是一个更高级的特性，且我们并不期望它成为通常开发过程的一部分，但是它的存在是至关重要的。如果 React Native 不支持你需要的本地特性，那么你应该能够自己构建它。

这是一个更高级的指南，展示了如何构建一个本地模块。它假设读者知道 Objective-C(Swift 还没有支持)和核心库(Foundation,UIKit)。

## iOS 日历模块的例子

本指南将使用 [iOS 日历 API 的例子](https://developer.apple.com/library/mac/documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)。假设我们希望能够从 JavaScript 访问 iOS 日历。

Native 模块只是一个 Objectve-C 类，实现了 `RCTBridgeModule` 协议。如果你想知道，RCT 是 ReaCT 的一个简称。


``` 
	// CalendarManager.h
	#import "RCTBridgeModule.h"
	#import "RCTLog.h"
	@interface CalendarManager : NSObject <RCTBridgeModule>
	@end
```

React Native 不会向 JavaScript 公开任何 `CalendarManager` 方法，除非有明确的要求。幸运的是有了 `RCT_EXPORT`，这会非常简单：


``` 
	// CalendarManager.m
	@implementation CalendarManager
	- (void)addEventWithName:(NSString *)name location:(NSString *)location
	{
  		RCT_EXPORT();
  		RCTLogInfo(@"Pretending to create an event %@ at %@", name, location);
	}
	@end
```

现在从你的 JavaScript 文件中，你可以像这样调用方法：


``` 
	var CalendarManager = require('NativeModules').CalendarManager;
	CalendarManager.addEventWithName('Birthday Party', '4 Privet Drive, Surrey');
```

注意，导出的方法名称是从 Objective-C 选择器的第一部分中生成的。有时它会产生一个非惯用的  JavaScript 名称(就像在我们的例子中的那个)。你可以通过为 `RCT_EXPORT` 提供一个可选参数更改名字，如 `RCT_EXPORT(addEvent)`。

方法返回的类型应该是 `void`。React Native 桥是异步的,所以向 JavaScript 传递结果的唯一方法是使用回调或 emitting 事件(见下文)。

## 参数类型

React Native 支持多种参数类型，可以从 JavaScript 代码传递到 native 模块： 

- 字符串型(`NSString`)
- 数字型(`NSInteger`，`float`,`double` ,`CGFloat`,` NSNumber`)
- 布尔型(`BOOL`,`NSNumber`)
- 这个列表中任何类型的数组(`NSArray`) 
- 这个列表中任何类型的字符串键和值的映射(`NSDictionary`)
- 函数(`RCTResponseSenderBlock`)

在我们的 `CalendarManager` 示例中,如果我们想把事件日期传递到 native，我们必须将它转换成一个字符串或一个数字：


``` 
	- (void)addEventWithName:(NSString *)name location:(NSString *)location date:(NSInteger)secondsSinceUnixEpoch
	{
  		RCT_EXPORT(addEvent);
  		NSDate *date = [NSDate dateWithTimeIntervalSince1970:secondsSinceUnixEpoch];
	}
```

随着 `CalendarManager.addEvent` 方法变得越来越复杂，参数的数量将会增加。其中一些可能是可选的。在这种情况下对改变 API 一点来接受事件属性的字典是值得考虑的，如：


```
	#import "RCTConvert.h"
	- (void)addEventWithName:(NSString *)name details:(NSDictionary *)details
	{
  		RCT_EXPORT(addEvent);
  		NSString *location = [RCTConvert NSString:details[@"location"]]; // ensure location is a string
  		...
	}
```

并且从 JavaScript 调用它：


``` 
	CalendarManager.addEvent('Birthday Party', {
  		location: '4 Privet Drive, Surrey',
  		time: date.toTime(),
  		description: '...'
	})
```


>**注意：关于数组和映射** 

>React Ntive 没有为这些结构中值的类型提供任何担保。你的 native 模块可能期望一个字符串数组，但如果 JavaScript 调用你的包含数字和字符串数组的方法，你会得到带有 ` NSNumber` 和 ` NSString` 的 `NSArray`。检查数组/映射值类型是开发人员的责任 (助手方法见 `RCTConvert`)。

## 回调

>**警告**

>本节比其他更具有实验性，围绕回调我们没有得到一组最佳实践。

Native 模块还支持一种特殊的参数——回调。在大多数情况下它是用来向 JavaScript 提供函数调用结果的。

``` 
	- (void)findEvents:(RCTResponseSenderBlock)callback
	{
  		RCT_EXPORT();
  		NSArray *events = ...
  		callback(@[[NSNull null], events]);
	}
```

`RCTResponseSenderBlock` 只接受一个参数——参数的数组传递给 JavaScript 的回调。在本例中，我们使用节点的惯例来为 error 和其他的——函数的结果设置第一个参数。

``` 
CalendarManager.findEvents((error, events) => {
  if (error) {
    console.error(error);
  } else {
    this.setState({events: events});
  }
})
```

Native 模块应该只调用它的回调一次。然而，它可以将回调作为 ivar 存储并稍后调用回调。这种模式通常用于包装需要委托的 iOS 的 APIs。请看 `RCTAlertManager`。

如果你想向 JavaScript 传递 error ——如对象，使用 `RCTUtils.h` 的 `RCTMakeError`。

## 实现 native 模块

Native 模块应该没有任何关于什么线程正在被调用的假设。React Native 在一个单独的串行 GCD 队列中调用 native 模块方法，但这是一个实现细节，可能会改变。如果 native 模块需要调用 main-thread-only iOS API，它应该在主队列安排操作：

``` 
- (void)addEventWithName:(NSString *)name callback:(RCTResponseSenderBlock)callback
{
  RCT_EXPORT(addEvent);
  dispatch_async(dispatch_get_main_queue(), ^{
    // Call iOS API on main thread
    ...
    // You can invoke callback from any thread/queue
    callback(@[...]);
  });
}
```

同样的方法，如果操作要很长时间才能完成，native 模块不应该阻塞。使用 `dispatch_async` 在后台队列中安排耗费大的工作是一个好主意。

## 导出常量

Native 模块可以在运行时向 JavaScript 导出立即可用的常量。导出一些初始数据是有用的，否则这些初始数据需要往返的桥梁。

``` 
- (NSDictionary *)constantsToExport
{
  return @{ @"firstDayOfTheWeek": @"Monday" };
}
```

JavaScript 能够立即使用这些值：

``` 
console.log(CalendarManager.firstDayOfTheWeek);
```

注意，只有在初始化时常量才能被导出，所以如果你在运行时改变了 `constantsToExport` 的值，它不会影响 JavaScript 环境。

## 发送事件到 JavaScript

Native 模块可以在不被直接调用的情况下向 JavaScript 发送事件信号。最简单的方法是使用 `eventDispatcher`：

``` 
	#import "RCTBridge.h"
	#import "RCTEventDispatcher.h"
	@implementation CalendarManager
	@synthesize bridge = _bridge;
	- (void)calendarEventReminderReceived:(NSNotification *)notification
	{
  		NSString *eventName = notification.userInfo[@"name"];
  		[self.bridge.eventDispatcher sendAppEventWithName:@"EventReminder"
                                               body:@{@"name": eventName}];
	}
	@end
```

JavaScript 代码可以订阅这些事件：

``` 
var subscription = DeviceEventEmitter.addListener(
  'EventReminder',
  (reminder) => console.log(reminder.name)
);
...
// Don't forget to unsubscribe
subscription.remove();
```

更多的向 JavaScript 发送事件的例子，请看 [RCTLocationObserver](https://github.com/facebook/react-native/blob/master/Libraries/Geolocation/RCTLocationObserver.m)。
