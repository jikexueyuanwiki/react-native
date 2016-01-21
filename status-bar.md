# iOS 状态栏 

保留所有版权。

源代码是在 BSD-style 许可下经过许可的，是在源代码树的根目录下的LICENSE 文件里找到的。额外授予的专利权可以在同目录下的 PATENTS 文件里找到。

@flow

## 方法  

static **setStyle**(style: number, animated?: boolean) 

static **setHidden**(hidden: boolean, animation: number) 
 
## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/StatusBarIOSExample.js)

```
    'use strict';
	var React = require('react-native');
	var {
	  StyleSheet,
	  View,
	  Text,
	  TouchableHighlight,
	  StatusBarIOS,
	} = React;
	exports.framework = 'React';
	exports.title = 'StatusBarIOS';
	exports.description = 'Module for controlling iOS status bar';
	exports.examples = [{
	  title: 'Status Bar Style',
	  render() {
	    return (
	      <View>
	        {Object.keys(StatusBarIOS.Style).map((key) =>
	          <TouchableHighlight style={styles.wrapper}
 	           onPress={() => StatusBarIOS.setStyle(StatusBarIOS.Style[key])}>
	            <View style={styles.button}>
	              <Text>setStyle(StatusBarIOS.Style.{key})</Text>
	            </View>
	          </TouchableHighlight>
	        )}
	      </View>
	    );
	  },
	}, {
	  title: 'Status Bar Style Animated',
	  render() {
	    return (
	      <View>
	        {Object.keys(StatusBarIOS.Style).map((key) =>
 	         <TouchableHighlight style={styles.wrapper}
	            onPress={() => StatusBarIOS.setStyle(StatusBarIOS.Style[key], true)}>
	            <View style={styles.button}>
	              <Text>setStyle(StatusBarIOS.Style.{key}, true)</Text>
	            </View>
	          </TouchableHighlight>
	        )}
	      </View>
	    );
	  },
	}, {
	  title: 'Status Bar Hidden',
	  render() {
	    return (
	      <View>
 	       {Object.keys(StatusBarIOS.Animation).map((key) =>
	          <View>
	            <TouchableHighlight style={styles.wrapper}
	              onPress={() => StatusBarIOS.setHidden(true, StatusBarIOS.Animation[key])}>
	              <View style={styles.button}>
	                <Text>setHidden(true, StatusBarIOS.Animation.{key})</Text>
	              </View>
	            </TouchableHighlight>
	            <TouchableHighlight style={styles.wrapper}
	              onPress={() => StatusBarIOS.setHidden(false, StatusBarIOS.Animation[key])}>
	              <View style={styles.button}>
	                <Text>setHidden(false, StatusBarIOS.Animation.{key})</Text>
	              </View>
	            </TouchableHighlight>
	          </View>
	        )}
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