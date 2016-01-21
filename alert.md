# iOS 警告 

利用一个特定的标题和消息来启动一个警告对话框。

提供一个可选按钮的列表。点击任何按钮触发各自的按下回调动作，并且忽略警告。在默认情况下，只有一个按钮是“OK”按钮。列表中最后一个按钮被视为“主”按钮，它被用粗体显示出来了。

```
    AlertIOS.alert(
	  'Foo Title',
	  'My Alert Msg',
	  [
	    {text: 'Foo', onPress: () => console.log('Foo Pressed!')},
	    {text: 'Bar', onPress: () => console.log('Bar Pressed!')},
	  ]
    )}
```

## 方法 

```
static alert(title: string, message?: string, buttons?: Array<{ text: ?string; onPress: ?Function; }>)  
```

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/AlertIOSExample.js)

```
    'use strict';
	var React = require('react-native');
	var {
	  StyleSheet,
	  View,
	  Text,
	  TouchableHighlight,
	  AlertIOS,
	} = React;
	exports.framework = 'React';
	exports.title = 'AlertIOS';
	exports.description = 'iOS alerts and action sheets';
	exports.examples = [{
	  title: 'Alerts',
	  render() {
	    return (
	      <View>
	        <TouchableHighlight style={styles.wrapper}
	          onPress={() => AlertIOS.alert(
	            'Foo Title',
	            'My Alert Msg'
	          )}>
	          <View style={styles.button}>
	            <Text>Alert with message and default button</Text>
	          </View>
	        </TouchableHighlight>
	        <TouchableHighlight style={styles.wrapper}
	          onPress={() => AlertIOS.alert(
	            null,
	            null,
	            [
	              {text: 'Button', onPress: () => console.log('Button Pressed!')},
	            ]
	          )}>
	          <View style={styles.button}>
	            <Text>Alert with only one button</Text>
	          </View>
	        </TouchableHighlight>
	        <TouchableHighlight style={styles.wrapper}
	          onPress={() => AlertIOS.alert(
	            'Foo Title',
	            'My Alert Msg',
	            [
	              {text: 'Foo', onPress: () => console.log('Foo Pressed!')},
	              {text: 'Bar', onPress: () => console.log('Bar Pressed!')},
	            ]
	          )}>
	          <View style={styles.button}>
	            <Text>Alert with two buttons</Text>
	          </View>
	        </TouchableHighlight>
	        <TouchableHighlight style={styles.wrapper}
	          onPress={() => AlertIOS.alert(
	            'Foo Title',
	            null,
	            [
	              {text: 'Foo', onPress: () => console.log('Foo Pressed!')},
	              {text: 'Bar', onPress: () => console.log('Bar Pressed!')},
	              {text: 'Baz', onPress: () => console.log('Baz Pressed!')},
	            ]
	          )}>
	          <View style={styles.button}>
	            <Text>Alert with 3 buttons</Text>
	          </View>
	        </TouchableHighlight>
	        <TouchableHighlight style={styles.wrapper}
	          onPress={() => AlertIOS.alert(
	            'Foo Title',
	            'My Alert Msg',
	            '..............'.split('').map((dot, index) => ({
	              text: 'Button ' + index,
	              onPress: () => console.log('Pressed ' + index)
	            }))
	          )}>
	          <View style={styles.button}>
	            <Text>Alert with too many buttons</Text>
	          </View>
	        </TouchableHighlight>
	      </View>
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