# iOS 导航器

iOS 导航器包装了 UIKit 导航，并且允许你添加跨应用程序的 back-swipe 功能。

### 路线

路线是用于描述导航器每个页面的一个对象。第一个提供给 NavigatorIOS 的路线是  `initialRoute`：

``` 
render: function() {
  return (
    <NavigatorIOS
      initialRoute={{
        component: MyView,
        title: 'My View Title',
        passProps: { myProp: 'foo' },
      }}
    />
  );
},
```

现在将由导航器呈现 MyView。它将在 `route` 道具，导航器及所有的 `passProps` 指定的道具中接受一个路线对象。

路线完整的定义请看 initialRoute propType。

### 导航器

`Navigator` 是视图能够调用的导航函数的一个对象。它作为一个道具会被传递给任何由 NavigatorIOS 呈现的组件。

``` 
var MyView = React.createClass({
  _handleBackButtonPress: function() {
    this.props.navigator.pop();
  },
  _handleNextButtonPress: function() {
    this.props.navigator.push(nextRoute);
  },
  ...
});
```

一个导航对象包含以下功能:

- `push(route)`——导航到一个新的路线
- `pop()`——返回一个页面
- `popN(n)`——一次返回 N 页。当 N=1 时，该行为相当于 `pop()`
- `replace(route)`——取代当前页面的路线，并立即为新路线加载视图
- `replacePrevious(route)`——取代前一页的路线/视图
- `replacePreviousAndPop(route)`——取代了以前的路线/视图并转换回去
- `resetTo(route)`——取代顶级的项目并 popToTop
- `popToRoute(route)`——为特定的路线对象回到项目
- `popToTop()`——回到顶级项目

导航功能在 NavigatorIOS 组件中也是可用的：

``` 
var MyView = React.createClass({
  _handleNavigationRequest: function() {
    this.refs.nav.push(otherRoute);
  },
  render: () => (
    <NavigatorIOS
      ref="nav"
      initialRoute={...}
    />
  ),
});
```

## Props  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/Navigation/NavigatorIOS.ios.js)

**initialRoute** {组件：函数型，标题：字符串型，passProps：对象型，backButtonTitle：字符串型，rightButtonTitle：字符串型，onRightButtonPress：函数型，wrapperStyle：[对象型Object]}

NavigatorIOS 使用“路线”对象来识别子视图，道具，及导航栏的配置。“push”和所有其他的导航操作预计路线是这样的：

**itemWrapperStyle** [View#style](view.md#style)

默认的包为 navigator 中的组件设置样式。一个常见的用例是为每一页设置 backgroundColor

**tintColor** 字符串型

在导航栏中的按钮使用的颜色

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/NavigatorIOSExample.js)

``` 
'use strict';
var React = require('react-native');
var ViewExample = require('./ViewExample');
var createExamplePage = require('./createExamplePage');
var {
  PixelRatio,
  ScrollView,
  StyleSheet,
  Text,
  TouchableHighlight,
  View,
} = React;
var EmptyPage = React.createClass({
  render: function() {
    return (
      <View style={styles.emptyPage}>
        <Text style={styles.emptyPageText}>
          {this.props.text}
        </Text>
      </View>
    );
  },
});
var NavigatorIOSExample = React.createClass({
  statics: {
    title: '<NavigatorIOS>',
    description: 'iOS navigation capabilities',
  },
  render: function() {
    var recurseTitle = 'Recurse Navigation';
    if (!this.props.topExampleRoute) {
      recurseTitle += ' - more examples here';
    }
    return (
      <ScrollView style={styles.list}>
        <View style={styles.line}/>
        <View style={styles.group}>
          <View style={styles.row}>
            <Text style={styles.rowNote}>
              See &lt;UIExplorerApp&gt; for top-level usage.
            </Text>
          </View>
        </View>
        <View style={styles.line}/>
        <View style={styles.groupSpace}/>
        <View style={styles.line}/>
        <View style={styles.group}>
          {this._renderRow(recurseTitle, () => {
            this.props.navigator.push({
              title: NavigatorIOSExample.title,
              component: NavigatorIOSExample,
              backButtonTitle: 'Custom Back',
              passProps: {topExampleRoute: this.props.topExampleRoute || this.props.route},
            });
          })}
          {this._renderRow('Push View Example', () => {
            this.props.navigator.push({
              title: 'Very Long Custom View Example Title',
              component: createExamplePage(null, ViewExample),
            });
          })}
          {this._renderRow('Custom Right Button', () => {
            this.props.navigator.push({
              title: NavigatorIOSExample.title,
              component: EmptyPage,
              rightButtonTitle: 'Cancel',
              onRightButtonPress: () => this.props.navigator.pop(),
              passProps: {
                text: 'This page has a right button in the nav bar',
              }
            });
          })}
          {this._renderRow('Pop', () => {
            this.props.navigator.pop();
          })}
          {this._renderRow('Pop to top', () => {
            this.props.navigator.popToTop();
          })}
          {this._renderRow('Replace here', () => {
            var prevRoute = this.props.route;
            this.props.navigator.replace({
              title: 'New Navigation',
              component: EmptyPage,
              rightButtonTitle: 'Undo',
              onRightButtonPress: () => this.props.navigator.replace(prevRoute),
              passProps: {
                text: 'The component is replaced, but there is currently no ' +
                  'way to change the right button or title of the current route',
              }
            });
          })}
          {this._renderReplacePrevious()}
          {this._renderReplacePreviousAndPop()}
          {this._renderPopToTopNavExample()}
        </View>
        <View style={styles.line}/>
      </ScrollView>
    );
  },
  _renderPopToTopNavExample: function() {
    if (!this.props.topExampleRoute) {
      return null;
    }
    return this._renderRow('Pop to top NavigatorIOSExample', () => {
      this.props.navigator.popToRoute(this.props.topExampleRoute);
    });
  },
  _renderReplacePrevious: function() {
    if (!this.props.topExampleRoute) {
      // this is to avoid replacing the UIExplorerList at the top of the stack
      return null;
    }
    return this._renderRow('Replace previous', () => {
      this.props.navigator.replacePrevious({
        title: 'Replaced',
        component: EmptyPage,
        passProps: {
          text: 'This is a replaced "previous" page',
        },
        wrapperStyle: styles.customWrapperStyle,
      });
    });
  },
  _renderReplacePreviousAndPop: function() {
    if (!this.props.topExampleRoute) {
      // this is to avoid replacing the UIExplorerList at the top of the stack
      return null;
    }
    return this._renderRow('Replace previous and pop', () => {
      this.props.navigator.replacePreviousAndPop({
        title: 'Replaced and Popped',
        component: EmptyPage,
        passProps: {
          text: 'This is a replaced "previous" page',
        },
        wrapperStyle: styles.customWrapperStyle,
      });
    });
  },
  _renderRow: function(title: string, onPress: Function) {
    return (
      <View>
        <TouchableHighlight onPress={onPress}>
          <View style={styles.row}>
            <Text style={styles.rowText}>
              {title}
            </Text>
          </View>
        </TouchableHighlight>
        <View style={styles.separator} />
      </View>
    );
  },
});
var styles = StyleSheet.create({
  customWrapperStyle: {
    backgroundColor: '#bbdddd',
  },
  emptyPage: {
    flex: 1,
    paddingTop: 64,
  },
  emptyPageText: {
    margin: 10,
  },
  list: {
    backgroundColor: '#eeeeee',
    marginTop: 10,
  },
  group: {
    backgroundColor: 'white',
  },
  groupSpace: {
    height: 15,
  },
  line: {
    backgroundColor: '#bbbbbb',
    height: 1 / PixelRatio.get(),
  },
  row: {
    backgroundColor: 'white',
    justifyContent: 'center',
    paddingHorizontal: 15,
    paddingVertical: 15,
  },
  separator: {
    height: 1 / PixelRatio.get(),
    backgroundColor: '#bbbbbb',
    marginLeft: 15,
  },
  rowNote: {
    fontSize: 17,
  },
  rowText: {
    fontSize: 17,
    fontWeight: '500',
  },
});
module.exports = NavigatorIOSExample;
```
