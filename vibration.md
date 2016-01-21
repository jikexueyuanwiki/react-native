# iOS 震动 

震动 API 是在 `VibrationIOS.vibrate()` 里显示的。在 iOS 上，调用这个函数可以出发一秒钟的振动。振动是异步的，所以这个方法会立即返回。

这对不支持振动的设备是没有任何影响的，例如，iOS 模拟器。

目前是不支持振动模式的。 

## 方法  

static **vibrate**()  

## 例子  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/VibrationIOSExample.js)
```
'use strict';
var React = require('react-native');
var {
  StyleSheet,
  View,
  Text,
  TouchableHighlight,
  VibrationIOS
} = React;
exports.framework = 'React';
exports.title = 'VibrationIOS';
exports.description = 'Vibration API for iOS';
exports.examples = [{
  title: 'VibrationIOS.vibrate()',
  render() {
    return (
      <TouchableHighlight
        style={styles.wrapper}
        onPress={() => VibrationIOS.vibrate()}>
        <View style={styles.button}>
          <Text>Vibrate</Text>
        </View>
      </TouchableHighlight>
    );
  },
}];
var styles = StyleSheet.create({
  wrapper: {
    borderRadius: 5,
    marginBottom: 5,
  },
  button: {
    backgroundColor: '#eeeeee',
    padding: 10,
  },
});
```