
# ToastAndroid

它揭示了如何将本地 ToastAndroid 模块作为一个 JS 模块。它有一个名为 `showText` 的函数，其拥有的参数如下所示：

1. 字符串消息：将文本传递给 toast 的字符串
2. int 持续期：toast 的持续期。可能是 `ToastAndroid.SHORT` 或 `ToastAndroid.LONG`

## 方法

static **show**(message: string, duration: number) 

## 性质

**SHORT**: MemberExpression 

**LONG**: MemberExpression 

## 示例

```java
'use strict';

var React = require('react-native');
var {
  StyleSheet,
  Text,
  ToastAndroid,
  TouchableWithoutFeedback
} = React;

var UIExplorerBlock = require('UIExplorerBlock');
var UIExplorerPage = require('UIExplorerPage');

var ToastExample = React.createClass({

  statics: {
    title: 'Toast Example',
    description: 'Toast Example',
  },

  getInitialState: function() {
    return {};
  },

  render: function() {
    return (
      <UIExplorerPage title="ToastAndroid">
        <UIExplorerBlock title="Simple toast">
          <TouchableWithoutFeedback
            onPress={() =>
              ToastAndroid.show('This is a toast with short duration', ToastAndroid.SHORT)}>
            <Text style={styles.text}>Click me.</Text>
          </TouchableWithoutFeedback>
        </UIExplorerBlock>
        <UIExplorerBlock title="Toast with long duration">
          <TouchableWithoutFeedback
            onPress={() =>
              ToastAndroid.show('This is a toast with long duration', ToastAndroid.LONG)}>
            <Text style={styles.text}>Click me too.</Text>
          </TouchableWithoutFeedback>
        </UIExplorerBlock>
      </UIExplorerPage>
    );
  },
});

var styles = StyleSheet.create({
  text: {
    color: 'black',
  },
});

module.exports = ToastExample;
```
