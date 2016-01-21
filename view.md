# 视图 

创建 UI 最基本的组件，`view` 是一个容器，它支持 flexbox 布局、风格、一些触发处理，和可访问性控制，它被设计成嵌套在其他视图里，并且有 0 到很多个任意类型的孩子。`view` 直接映射到原生视图，相当于在任意正在运行的平台上的响应，不管它是 `UIView`，`<div>`，`android.view`，等等。这个例子创建了一个视图，将两个颜色的框和自定义的组件打包填充成一行。

``` 
<View style={{flexDirection: 'row', height: 100, padding: 20}}>
  <View style={{backgroundColor: 'blue', flex: 0.3}} />
  <View style={{backgroundColor: 'red', flex: 0.5}} />
  <MyCustomComponent {...customProps} />
</View>
```

为了清晰和性能，视图被设计成与样式表一起使用，尽管是内联样式也同样支持。

## 工具 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/View/View.js)

**accessibilityLabel** 字符串型 

当用户与元素进行交互时，覆盖通过屏幕阅读器阅读的文本。在默认情况下，标签是通过遍历所有孩子和累积所有由空间隔开的文本节点创建的。 

**accessible** 布尔型 

当它的值为真时，说明视图是一个可访问的元素。在默认情况下，所有的可触发的元素都是可以被访问的。

**onMoveShouldSetResponder** 函数 

 对于大多数的触发交互，你可能只是想在 `TouchableHighlight` 或者 `TouchableOpacity` 里包装你的组件。为了进一步的探讨，检查 `Touchable.js`，`ScrollResponder.js` 和`ResponderEventPlugin.js`。 

**onResponderGrant** 函数  

**onResponderMove** 函数  

**onResponderReject** 函数  

**onResponderRelease** 函数 

**onResponderTerminate** 函数 

**onResponderTerminationRequest** 函数  

**onStartShouldSetResponder** 函数  

**onStartShouldSetResponderCapture** 函数 

**pointerEvents** enum('box-none', 'none', 'box-only', 'auto')  

缺乏`auto` 属性，`none` 更像是 `CSS` 的 `none` 值。`box-none` 就好像你已经应用了 `CSS` 类：

```
.box-none {
  pointer-events: none;
}
.box-none * {
  pointer-events: all;
}
```

`box-only` 相当于

``` 
.box-only {
  pointer-events: all;
}
.box-only * {
  pointer-events: none;
}
```

但是由于 `pointerEvents` 并不影响布局/外观，通过添加附加模式，我们已经偏离了规范，我们选择在 `style` 上不包括 `pointerEvents`。在一些平台上，不管怎样偶们都需要将它作为一个 `className` 来实现。是否使用 `style` 时这个平台的实现细节。

**removeClippedSubviews** 布尔 

这是一个通过 RCTView 显示的特定性能属性，当有很多子视图，并且它们大部分都是在幕后，这时被用于滚动内容。为了使这个属性有效，它必须被应用到一个视图中，在这个视图里包含很多子视图和外部约束。子视图中还应该有溢出：隐藏，应该包含视图（或者它的一个子视图）。 

<a name="style"></a>
**style** 字体

[**Flexbox...**](flexbox.md)

**backgroundColor** 字符串 

**borderBottomColor** 字符串 

**borderColor** 字符串

**borderLeftColor** 字符串

**borderRadius** 数值

**borderRightColor** 字符串

**borderTopColor** 字符串

**opacity** 数值

**overflow** enum('visible', 'hidden')

**rotation** 数值

**scaleX** 数值

**scaleY** 数值

**shadowColor** 字符串

**shadowOffset** {h: number, w: number}

**shadowOpacity** 数值

**shadowRadius** 数值

**transformMatrix** [数值]

**translateX** 数值

**translateY** 数值

**testID** 字符串型

在端到端测试中用于定位视图

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/ViewExample.js)

``` 
'use strict';
var React = require('react-native');
var {
  StyleSheet,
  Text,
  View,
} = React;
var styles = StyleSheet.create({
  box: {
    backgroundColor: '#527FE4',
    borderColor: '#000033',
    borderWidth: 1,
  }
});
exports.title = '<View>';
exports.description = 'Basic building block of all UI.';
exports.displayName = 'ViewExample';
exports.examples = [
  {
    title: 'Background Color',
    render: function() {
      return (
        <View style={{backgroundColor: '#527FE4', padding: 5}}>
          <Text style={{fontSize: 11}}>
            Blue background
          </Text>
        </View>
      );
    },
  }, {
    title: 'Border',
    render: function() {
      return (
        <View style={{borderColor: '#527FE4', borderWidth: 5, padding: 10}}>
          <Text style={{fontSize: 11}}>5px blue border</Text>
        </View>
      );
    },
  }, {
    title: 'Padding/Margin',
    render: function() {
      return (
        <View style={{borderColor: '#bb0000', borderWidth: 0.5}}>
          <View style={[styles.box, {padding: 5}]}>
            <Text style={{fontSize: 11}}>5px padding</Text>
          </View>
          <View style={[styles.box, {margin: 5}]}>
            <Text style={{fontSize: 11}}>5px margin</Text>
          </View>
          <View style={[styles.box, {margin: 5, padding: 5, alignSelf: 'flex-start'}]}>
            <Text style={{fontSize: 11}}>
              5px margin and padding,
            </Text>
            <Text style={{fontSize: 11}}>
              widthAutonomous=true
            </Text>
          </View>
        </View>
      );
    },
  }, {
    title: 'Border Radius',
    render: function() {
      return (
        <View style={{borderWidth: 0.5, borderRadius: 5, padding: 5}}>
          <Text style={{fontSize: 11}}>
            Too much use of `borderRadius` (especially large radii) on
            anything which is scrolling may result in dropped frames.
            Use sparingly.
          </Text>
        </View>
      );
    },
  }, {
    title: 'Circle with Border Radius',
    render: function() {
      return (
        <View style={{borderRadius: 10, borderWidth: 1, width: 20, height: 20}} />
      );
    },
  }, {
    title: 'Overflow',
    render: function() {
      return (
        <View style={{flexDirection: 'row'}}>
          <View
            style={{
              width: 95,
              height: 10,
              marginRight: 10,
              marginBottom: 5,
              overflow: 'hidden',
              borderWidth: 0.5,
            }}>
            <View style={{width: 200, height: 20}}>
              <Text>Overflow hidden</Text>
            </View>
          </View>
          <View style={{width: 95, height: 10, marginBottom: 5, borderWidth: 0.5}}>
            <View style={{width: 200, height: 20}}>
              <Text>Overflow visible</Text>
            </View>
          </View>
        </View>
      );
    },
  }, {
    title: 'Opacity',
    render: function() {
      return (
        <View>
          <View style={{opacity: 0}}><Text>Opacity 0</Text></View>
          <View style={{opacity: 0.1}}><Text>Opacity 0.1</Text></View>
          <View style={{opacity: 0.3}}><Text>Opacity 0.3</Text></View>
          <View style={{opacity: 0.5}}><Text>Opacity 0.5</Text></View>
          <View style={{opacity: 0.7}}><Text>Opacity 0.7</Text></View>
          <View style={{opacity: 0.9}}><Text>Opacity 0.9</Text></View>
          <View style={{opacity: 1}}><Text>Opacity 1</Text></View>
        </View>
      );
    },
  },
];
```
