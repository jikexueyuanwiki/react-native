# Native UI 组件(iOS)

有许多 native UI 小部件可以应用到最新的应用程序中——其中一些是平台的一部分，另外的可以用作第三方库，并且更多的是它们可以用于你自己的选集中。React Native 有几个最关键的平台组件已经包装好了，如 `ScrollView` 和 `TextInput`，但不是所有的组件都被包装好了，当然了，你为先前的应用程序写的组件肯定没有包装好。幸运的是，为了与 React Native 应用程序无缝集成，将现存组件包装起来是非常容易实现的。

正如 native 模块指南，这也是一种更高级的指南，假定你对 iOS 编程有一定的了解。本指南将向你展示如何构建一个本地的 UI 组件，带你实现在核心 React Native 库中可用的现存的 `MapView` 组件的子集。

## iOS MapView 示例

如果说我们想在我们的应用程序中添加一个交互式的 Map——不妨用 `MKMapView`，我们只需要让它在 JavaScript 中可用。

Native 视图是通过 `RCTViewManager` 的子类创建和操做的。这些子类的功能与视图控制器很相似，但本质上它们是单件模式——桥只为每一个子类创建一个实例。它们将 native 视图提供给 `RCTUIManager`，它会传回到 native 视图来设置和更新的必要的视图属性。`RCTViewManager` 通常也是视图的代表，通过桥将事件发送回 JavaScript。

发送视图是很简单的：

- 创建基本的子类。

- 添加标记宏 `RCT_EXPORT_MODULE()`。

- 实现 `-(UIView *)view` 方法。



``` 
// RCTMapManager.m
#import <MapKit/MapKit.h>
#import "RCTViewManager.h"
@interface RCTMapManager : RCTViewManager
@end
@implementation RCTMapManager
RCT_EXPORT_MODULE()
- (UIView *)view
{
  return [[MKMapView alloc] init];
}
@end
```

然后你需要一些 JavaScript 使之成为有用的 React 组件：

``` 
// MapView.js
var { requireNativeComponent } = require('react-native');
module.exports = requireNativeComponent('RCTMap', null);
```

现在这是 JavaScript 中一个功能完整的 native map 视图组件了，包括 pinch-zoom 和其他 native 手势支持。但是我们还不能用 JavaScript 来真正的控制它。

## 属性

为了使该组件更可用，我们可以做的第一件事是连接一些 native 属性。比如说我们希望能够禁用音高控制并指定可见区域。禁用音高是一个简单的布尔值，所以我们只添加这一行:

```
// RCTMapManager.m
RCT_EXPORT_VIEW_PROPERTY(pitchEnabled, BOOL)
```

注意我们显式的指定类型为 `BOOL`——当谈到连接桥时，React Native 使用 hood 下的 `RCTConvert` 来转换所有不同的数据类型，且错误的值会显示明显的 “RedBox” 错误使你知道这里有 ASAP 问题。当一切进展顺利时，这个宏就会为你处理整个实现。

现在要真正的实现禁用音高，我们只需要在 JS 中设置如下所示属性：

``` 
// MyApp.js
<MapView pitchEnabled={false} />
```

但是这不是很好记录——为了知道哪些属性可用以及它们接收了什么值，你的新组件的客户端需要挖掘 objective-C 代码。为了更好的实现这一点，让我们做一个包装器组件并用 React `PropTypes` 记录接口：

``` 
// MapView.js
var React = require('react-native');
var { requireNativeComponent } = React;
class MapView extends React.Component {
  render() {
    return <RCTMap {...this.props} />;
  }
}
var RCTMap = requireNativeComponent('RCTMap', MapView);
MapView.propTypes = {
  /**
   * When this property is set to `true` and a valid camera is associated
   * with the map, the camera’s pitch angle is used to tilt the plane
   * of the map. When this property is set to `false`, the camera’s pitch
   * angle is ignored and the map is always displayed as if the user
   * is looking straight down onto it.
   */
  pitchEnabled: React.PropTypes.bool,
};
module.exports = MapView;
```

现在我们有一个很不错的已记录的包装器组件，它使用非常容易。注意我们为新的 `MapView` 包装器组件将第二个参数从 `null` 改为 `requireNativeComponent`。这使得基础设施验证了 propTypes 匹配native 工具来减少 ObjC 和 JS 代码之间的不匹配的可能。

接下来，让我们添加更复杂的 `region` 工具。从添加 native 代码入手：

``` 
// RCTMapManager.m
RCT_CUSTOM_VIEW_PROPERTY(region, MKCoordinateRegion, RCTMap)
{
  [view setRegion:json ? [RCTConvert MKCoordinateRegion:json] : defaultView.region animated:YES];
}
```

好的，这显然比之前简单的 `BOOL` 情况更加复杂。现在我们有一个 `MKCoordinateRegion` 类型，该类型需要一个转换函数，并且我们有自定义的代码，这样当我们从 JS 设置区域时，视图可以产生动画效果。还有一个 `defaultView`，如果 JS 发送给我们一个 null 标记，我们使用它将属性重置回默认值。

当然你可以为你的视图编写任何你想要的转换函数——下面是通过 `RCTConvert` 的两类来实现 `MKCoordinateRegion` 的例子：

``` 
@implementation RCTConvert(CoreLocation)
RCT_CONVERTER(CLLocationDegrees, CLLocationDegrees, doubleValue);
RCT_CONVERTER(CLLocationDistance, CLLocationDistance, doubleValue);
+ (CLLocationCoordinate2D)CLLocationCoordinate2D:(id)json
{
  json = [self NSDictionary:json];
  return (CLLocationCoordinate2D){
    [self CLLocationDegrees:json[@"latitude"]],
    [self CLLocationDegrees:json[@"longitude"]]
  };
}
@end
@implementation RCTConvert(MapKit)
+ (MKCoordinateSpan)MKCoordinateSpan:(id)json
{
  json = [self NSDictionary:json];
  return (MKCoordinateSpan){
    [self CLLocationDegrees:json[@"latitudeDelta"]],
    [self CLLocationDegrees:json[@"longitudeDelta"]]
  };
}
+ (MKCoordinateRegion)MKCoordinateRegion:(id)json
{
  return (MKCoordinateRegion){
    [self CLLocationCoordinate2D:json],
    [self MKCoordinateSpan:json]
  };
}
```

这些转换函数是为了安全地处理任何 JSON 而设计的，当出现丢失的键或开发人员错误操作时，JS 可能向它们抛出 “RedBox” 错误并返回标准的初始化值。

为完成对 `region` 工具的支持，我们需要把它记录到 `propTypes`中(否则我们将得到一个错误，即 native 工具没有被记录)，然后我们就可以按照设置其他工具的方式来设置它：

``` 
// MapView.js
MapView.propTypes = {
  /**
   * When this property is set to `true` and a valid camera is associated
   * with the map, the camera’s pitch angle is used to tilt the plane
   * of the map. When this property is set to `false`, the camera’s pitch
   * angle is ignored and the map is always displayed as if the user
   * is looking straight down onto it.
   */
  pitchEnabled: React.PropTypes.bool,
  /**
   * The region to be displayed by the map.
   *
   * The region is defined by the center coordinates and the span of
   * coordinates to display.
   */
  region: React.PropTypes.shape({
    /**
     * Coordinates for the center of the map.
     */
    latitude: React.PropTypes.number.isRequired,
    longitude: React.PropTypes.number.isRequired,
    /**
     * Distance between the minimum and the maximum latitude/longitude
     * to be displayed.
     */
    latitudeDelta: React.PropTypes.number.isRequired,
    longitudeDelta: React.PropTypes.number.isRequired,
  }),
};
// MyApp.js
  render() {
    var region = {
      latitude: 37.48,
      longitude: -122.16,
      latitudeDelta: 0.1,
      longitudeDelta: 0.1,
    };
    return <MapView region={region} />;
  }
``` 

在这里你可以看到该区域的形状在 JS 文档中是显式的——理想情况下我们可以生成一些这方面的东西，但是这没有实现。

## 事件

所以现在我们有一个 native map 组件，可以从 JS 很容易的控制，但是我们如何处理来自用户的事件，如 pinch-zooms 或平移来改变可见区域？关键是要使 `RCTMapManager` 成为它发送的所有视图的代表，并把事件通过事件调度器发送给 JS。这看起来如下所示(从整个实现中简化出来的部分)：


``` 
// RCTMapManager.m
#import "RCTMapManager.h"
#import <MapKit/MapKit.h>
#import "RCTBridge.h"
#import "RCTEventDispatcher.h"
#import "UIView+React.h"
@interface RCTMapManager() <MKMapViewDelegate>
@end
@implementation RCTMapManager
RCT_EXPORT_MODULE()
- (UIView *)view
{
  MKMapView *map = [[MKMapView alloc] init];
  map.delegate = self;
  return map;
}
#pragma mark MKMapViewDelegate
- (void)mapView:(RCTMap *)mapView regionDidChangeAnimated:(BOOL)animated
{
  MKCoordinateRegion region = mapView.region;
  NSDictionary *event = @{
    @"target": mapView.reactTag,
    @"region": @{
      @"latitude": @(region.center.latitude),
      @"longitude": @(region.center.longitude),
      @"latitudeDelta": @(region.span.latitudeDelta),
      @"longitudeDelta": @(region.span.longitudeDelta),
    }
  };
  [self.bridge.eventDispatcher sendInputEventWithName:@"topChange" body:event];
}
```


你可以看到我们设置管理器为它发送的每个视图的代表，然后在代表方法 `-mapView:regionDidChangeAnimated:` 中，区域与 `reactTag` 目标相结合来产生事件，通过 `sendInputEventWithName:body` 分派到你应用程序中相应的 React 组件实例中。事件名称 `@"topChange"` 映射到从 JavaScript 中回调的 `onChange`（[这里](https://github.com/facebook/react-native/blob/master/React/Modules/RCTUIManager.m#L1146)查看 mappings ）。原始事件调用这个回调，我们通常在包装器组件中处理这个过程来实现一个简单的 API：

``` 
// MapView.js
class MapView extends React.Component {
  constructor() {
    this._onChange = this._onChange.bind(this);
  }
  _onChange(event: Event) {
    if (!this.props.onRegionChange) {
      return;
    }
    this.props.onRegionChange(event.nativeEvent.region);
  }
  render() {
    return <RCTMap {...this.props} onChange={this._onChange} />;
  }
}
MapView.propTypes = {
  /**
   * Callback that is called continuously when the user is dragging the map.
   */
  onRegionChange: React.PropTypes.func,
  ...
}; 
```

## 样式

由于我们所有的 native react 视图是 `UIView` 的子类，大多数样式属性会像你预想的一样内存不足。然而，一些组件需要默认的样式，例如 `UIDatePicker`，大小固定。为了达到预期的效果，默认样式对布局算法来说是非常重要的，但是我们也希望在使用组件时能够覆盖默认的样式。`DatePickerIOS` 通过包装一个额外的视图中的 native 组件实现这一功能，该额外的视图具有灵活的样式设计，并在内部 native 组件中使用一个固定的样式(用从 native 传递的常量生成)：

``` 
// DatePickerIOS.ios.js
var RCTDatePickerIOSConsts = require('NativeModules').UIManager.RCTDatePicker.Constants;
...
  render: function() {
    return (
      <View style={this.props.style}>
        <RCTDatePickerIOS
          ref={DATEPICKER}
          style={styles.rkDatePickerIOS}
          ...
        />
      </View>
    );
  }
});
var styles = StyleSheet.create({
  rkDatePickerIOS: {
    height: RCTDatePickerIOSConsts.ComponentHeight,
    width: RCTDatePickerIOSConsts.ComponentWidth,
  },
});
```

`RCTDatePickerIOSConsts` 常量是通过抓取 native 组件的实际框架从 native 中导出的，如下所示：


``` 
// RCTDatePickerManager.m
- (NSDictionary *)constantsToExport
{
  UIDatePicker *dp = [[UIDatePicker alloc] init];
  [dp layoutIfNeeded];
  return @{
    @"ComponentHeight": @(CGRectGetHeight(dp.frame)),
    @"ComponentWidth": @(CGRectGetWidth(dp.frame)),
    @"DatePickerModes": @{
      @"time": @(UIDatePickerModeTime),
      @"date": @(UIDatePickerModeDate),
      @"datetime": @(UIDatePickerModeDateAndTime),
    }
  };
}
```

本指南涵盖了衔接自定义 native 组件的许多方面，但有你可能有更多需要考虑的地方，如自定义 hooks 来插入和布局子视图。如果你想了解更多，请在[源代码](https://github.com/facebook/react-native/blob/master/React/Views)中查看实际的 `RCTMapManager` 和其他组件。