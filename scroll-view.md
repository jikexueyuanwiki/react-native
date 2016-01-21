# 滚动视图

组件封装了滚动视图平台，同时提供了与锁定“应答”系统的触摸的集成。

尚不支持其他来自阻止滚动视图成为响应者的包含的响应。

## Props 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/ScrollView/ScrollView.js) 

**alwaysBounceHorizontal** 布尔型 

当为真时，滚动视图到达内容底部时，水平反弹，即使该内容小于滚动视图。当 `horizontal={true} `时，默认值为 true，否则，默认值为 false。

**alwaysBounceVertical** 布尔型

当为真时，滚动视图到达内容底部时，垂直反弹，即使该内容小于滚动视图。当 `horizontal={true}` 时，默认值为 false，否则，默认值为 true。

**automaticallyAdjustContentInsets** 布尔型

**bounces** 布尔型

当为真时，当滚动视图到达内容底部时，反弹，如果内容比滚动视图是大，那么滚动视图沿着轴滚动方向反弹。当为假时，禁用所有反弹，即使 `alwaysBounce *` 道具为真。默认值为 true。

**centerContent bool** 布尔型

当为真时，当内容小于滚动视图边界时，滚动视图自动的集中内容；当内容大于滚动视图时,该属性没有任何影响。默认值是 false。

**contentContainerStyle** StyleSheetPropType(ViewStylePropTypes)

这些样式将应用到滚动视图内容容器中，内容容器包装了所有的子视图。例如：

```
return ( <ScrollView contentContainerStyle={styles.contentContainer}> </ScrollView> ); ... var styles = StyleSheet.create({ contentContainer: { paddingVertical: 20 } });
```

**contentInset** {顶部：数字型，左部：数字型，底部：数字型，右部：数字型}

**contentOffset** PointPropType

**decelerationRate** 数字型

一个浮点数，决定了在用户移开手指后，滚动视图减速有多快。合理的选择包括——正常：0.998(默认)——快速：0.9

**horizontal** 布尔型

当为真时，滚动视图的子视图水平排列为一行，而不是竖直排列为一列。默认值是 false。

**keyboardDismissMode** 枚举型(“none”，“interactive”，“onDrag”)

确定键盘在响应一个拖动时是否被摒弃。——“none”(默认)，拖动没有摒弃键盘。——“onDrag”，当拖动开始时键盘就被摒弃了。——“interactive”，键盘被拖动交互式地摒弃并且与触摸同步移动；向上拖动取消了摒弃。

**keyboardShouldPersistTaps** 布尔型

当为假时，当键盘向上摒弃键盘时，轻击外部关注文本输入。当为真时，滚动视图不会抓取轻击，键盘不会自动摒弃。默认值是 false。

**maximumZoomScale** 数字型

最大允许缩放比例。默认值是 1.0。

**minimumZoomScale** 数字型

最小允许缩放比例。默认值是 1.0。

**onScroll** 函数型

**onScrollAnimationEnd** 函数型

**pagingEnabled** 布尔型

当为真时，滚动视图在滚动时会在滚动视图的尺寸的倍数上停止滚动。这可以用于水平分页。默认值是 false。

**removeClippedSubviews** 布尔型

实验：当为真时，屏幕以外的子视图(它的 `overflow` 值是 `hidden )从本地备份的 superview 中删除。这在长列表中可以提高滚动性能。默认值是 false。

**scrollEnabled** 布尔型

**scrollEventThrottle** 数字型

**scrollIndicatorInsets** {顶部：数字型，左部：数字型，底部：数字型，右部：数字型}

**scrollsToTop** 布尔型

当为真时，轻击状态栏滚动视图会滚动到顶部。默认值为 true。

**showsHorizontalScrollIndicator** 布尔型

**showsVerticalScrollIndicator** 布尔型

**stickyHeaderIndices** [数字型]

一组子视图表明确定当视图滚动时哪些子视图会停靠在屏幕的顶端。例如，传递 `stickyHeaderIndices = {[0]}` 将使得第一个子视图固定在滚动视图的顶部。此属性不支持与 `horizontal = {true}` 结合。

**style** style

[Flexbox...](flexbox.md)

**backgroundColor** 字符串型

**borderBottomColor** 字符串型

**borderColor** 字符串型

**borderLeftColor** 字符串型

**borderRadius** 数字型

**borderRightColor** 字符串型

**borderTopColor** 字符串型

**opacity** 数字型

**overflow** 枚举型(‘visible’,’hidden’)

**rotation** 数字型

**scaleX** 数字型

**scaleY** 数字型

**shadowColor** 字符串型

**shadowOffset** {高：数字型，宽：数字型}

**shadowOpacity** 数字型

**shadowRadius** 数字型

**transformMatrix** [数字型]

**translateX** 数字型

**translateY** 数字型

**zoomScale** 数字型

当前滚动视图内容的规模。默认值是 1.0。

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/ScrollViewExample.js)

``` 
'use strict';
var React = require('react-native');
var {
  ScrollView,
  StyleSheet,
  View,
  Image
} = React;
exports.title = '<ScrollView>';
exports.description = 'Component that enables scrolling through child components';
exports.examples = [
{
  title: '<ScrollView>',
  description: 'To make content scrollable, wrap it within a <ScrollView> component',
  render: function() {
    return (
      <ScrollView
        onScroll={() => { console.log('onScroll!'); }}
        scrollEventThrottle={200}
        contentInset={{top: -50}}
        style={styles.scrollView}>
        {THUMBS.map(createThumbRow)}
      </ScrollView>
    );
  }
}, {
  title: '<ScrollView> (horizontal = true)',
  description: 'You can display <ScrollView>\'s child components horizontally rather than vertically',
  render: function() {
    return (
      <ScrollView
        horizontal={true}
        contentInset={{top: -50}}
        style={[styles.scrollView, styles.horizontalScrollView]}>
        {THUMBS.map(createThumbRow)}
      </ScrollView>
    );
  }
}];
var Thumb = React.createClass({
  shouldComponentUpdate: function(nextProps, nextState) {
    return false;
  },
  render: function() {
    return (
      <View style={styles.button}>
        <Image style={styles.img} source={{uri:this.props.uri}} />
      </View>
    );
  }
});
var THUMBS = ['https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-ash3/t39.1997/p128x128/851549_767334479959628_274486868_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851561_767334496626293_1958532586_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-ash3/t39.1997/p128x128/851579_767334503292959_179092627_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851589_767334513292958_1747022277_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851563_767334559959620_1193692107_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851593_767334566626286_1953955109_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851591_767334523292957_797560749_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851567_767334529959623_843148472_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851548_767334489959627_794462220_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851575_767334539959622_441598241_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-ash3/t39.1997/p128x128/851573_767334549959621_534583464_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851583_767334573292952_1519550680_n.png'];
THUMBS = THUMBS.concat(THUMBS); // double length of THUMBS
var createThumbRow = (uri, i) => <Thumb key={i} uri={uri} />;
var styles = StyleSheet.create({
  scrollView: {
    backgroundColor: '#6A85B1',
    height: 300,
  },
  horizontalScrollView: {
    height: 120,
  },
  containerPage: {
    height: 50,
    width: 50,
    backgroundColor: '#527FE4',
    padding: 5,
  },
  text: {
    fontSize: 20,
    color: '#888888',
    left: 80,
    top: 20,
    height: 40,
  },
  button: {
    margin: 7,
    padding: 5,
    alignItems: 'center',
    backgroundColor: '#eaeaea',
    borderRadius: 3,
  },
  buttonContents: {
    flexDirection: 'row',
    width: 64,
    height: 64,
  },
  img: {
    width: 64,
    height: 64,
  }
});
```