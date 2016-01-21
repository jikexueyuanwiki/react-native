# iOS 滑块 

## 属性   

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/SliderIOS/SliderIOS.js)   

**maximumValue** 数值型 

滑动块初始化最大值。默认值是 1。

**minimumValue** 数值型

滑动块初始化最小值。默认值是 0。

**onSlidingComplete** 函数

当用户已经完成改变它的值后，调用回调函数（例如，当滑动块被释放）

**onValueChange** 函数

当用户拖动滑动块时，连续不断的调用回调函数

**style** [View#style](view.md#style)

用于对 `Slider` 的设计与布局。未获取更多的信息，请查看`StyleSheet.js` 和 `ViewStylePropTypes.js`

**value** 数值型

初始化滑动块的值。该值应该是介于最大值和最小值之间的，最大值默认为 1，最小值默认为 0。默认值为 0。

这不是一个控制组件，比如说，如果你不更新组件的值，那么它将不会被重置成它的初始值。

## 例子

[Edit on GItHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/SliderIOSExample.js)

``` 
'use strict';
var React = require('react-native');
var {
  SliderIOS,
  Text,
  StyleSheet,
  View,
} = React;
var SliderExample = React.createClass({
  getInitialState() {
    return {
      value: 0,
    };
  },
  render() {
    return (
      <View>
        <Text style={styles.text} >
          {this.state.value}
        </Text>
        <SliderIOS
          style={styles.slider}
          onValueChange={(value) => this.setState({value: value})} />
      </View>
    );
  }
});
var styles = StyleSheet.create({
  slider: {
    height: 10,
    margin: 10,
  },
  text: {
    fontSize: 14,
    textAlign: 'center',
    fontWeight: '500',
    margin: 10,
  },
});
exports.title = '<SliderIOS>';
exports.displayName = 'SliderExample';
exports.description = 'Slider input for numeric values';
exports.examples = [
  {
    title: 'SliderIOS',
    render(): ReactElement { return <SliderExample />; }
  }
];
```
