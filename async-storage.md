# 异步存储 

异步存储是一个简单的、异步的、持久的、全局的、键-值存储系统。它应该会代替本地存储被使用。

由于异步存储是全局性的，建议您在异步存储之上使用抽象体，而不是对任何轻微用法直接使用异步存储。

在本地 iOS 实现上 JS 代码是一个简单的外观模式，用来提供一个清晰的 JS API，真正的错误对象，和简单的非多元化功能。每个方法返回一个 `Promise` 对象。 

## 方法

```
static **getItem**(key: string, callback: (error: ?Error, result: ?string) => void) 
```

如果有任何一个错误，获取 `key` 并传递 `callback` 的结果，返回一个 `Promise` 对象。

```
static **setItem**(key: string, value: string, callback: ?(error: ?Error) => void)
```

如果有任何一个错误，获取 `key` 并在结束时调用 `callback` 函数，返回一个 `Promise` 对象。

```
static **removeItem**(key: string, callback: ?(error: ?Error) => void)
```

返回一个 `Promise` 对象。

```
static **mergeItem**(key: string, value: string, callback: ?(error: ?Error) => void)
```

将现有值与输入值进行合并，假设它们是 stringified json，返回一个 `Promise` 对象。

所有本地实现不支持。

```
static **clear**(callback: ?(error: ?Error) => void)
```

为所有客户、函数库等清除所有的异步存储。你可能不想调用这个-使用 removeItem 或者 multiRemove 来清除只属于你的键值。返回一个  `Promise` 对象。

```
static **getAllKeys**(callback: (error: ?Error) => void)
```

为调用者、函数库等获取系统已知的所有键值。返回一个 `Promise` 对象。

```
static **multiGet**(keys: Array<string>, callback: (errors: ?Array<Error>, result: ?Array<Array<string>>) => void)

multiGet利用一个键值对的数组调用回调函数来获取multiSet的输入格式。返回一个 `Promise` 对象。

multiGet(['k1', 'k2'], cb) -> cb([['k1', 'val1'], ['k2', 'val2']])

static **multiSet**(keyValuePairs: Array<Array<string>>, callback: ?(errors: ?Array<Error>) => void)
```

multiSet 和 multiMerge 利用键值对的数组匹配multiGet的输出。返回一个 `Promise` 对象。例如，

```
multiSet([['k1', 'val1'], ['k2', 'val2']], cb);

static **multiRemove**(keys: Array<string>, callback: ?(errors: ?Array<Error>) => void) 
```

删除键值数组中所有的键值。返回一个 `Promise` 对象。

```
static **multiMerge**(keyValuePairs: Array<Array<string>>, callback: ?(errors: ?Array<Error>) => void)
```

将现有值与输入值进行合并，假设它们是 stringified json，返回一个 `Promise` 对象。

所有本地实现不支持。 

## 例子

[dit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/AsyncStorageExample.js)

```
    'use strict';
	var React = require('react-native');
	var {
	  AsyncStorage,
	  PickerIOS,
	  Text,
	  View
	} = React;
	var PickerItemIOS = PickerIOS.Item;
	var STORAGE_KEY = '@AsyncStorageExample:key';
	var COLORS = ['red', 'orange', 'yellow', 'green', 'blue'];
	var BasicStorageExample = React.createClass({
	  componentDidMount() {
	    AsyncStorage.getItem(STORAGE_KEY)
	      .then((value) => {
	        if (value !== null){
	          this.setState({selectedValue: value});
	          this._appendMessage('Recovered selection from disk: ' + value);
	        } else {
	          this._appendMessage('Initialized with no selection on disk.');
	        }
	      })
	      .catch((error) => this._appendMessage('AsyncStorage error: ' + error.message))
	      .done();
	  },
	  getInitialState() {
	    return {
	      selectedValue: COLORS[0],
	      messages: [],
	    };
	  },
	  render() {
	    var color = this.state.selectedValue;
	    return (
	      <View>
	        <PickerIOS
	          selectedValue={color}
	          onValueChange={this._onValueChange}>
	          {COLORS.map((value) => (
	            <PickerItemIOS
	              key={value}
	              value={value}
	              label={value}
	            />
	          ))}
	        </PickerIOS>
	        <Text>
	          {'Selected: '}
	          <Text style={{color}}>
	            {this.state.selectedValue}
	          </Text>
	        </Text>
	        <Text>{' '}</Text>
	        <Text onPress={this._removeStorage}>
	          Press here to remove from storage.
	        </Text>
	        <Text>{' '}</Text>
	        <Text>Messages:</Text>
	        {this.state.messages.map((m) => <Text>{m}</Text>)}
	      </View>
	    );
	  },
	  _onValueChange(selectedValue) {
	    this.setState({selectedValue});
	    AsyncStorage.setItem(STORAGE_KEY, selectedValue)
	      .then(() => this._appendMessage('Saved selection to disk: ' + selectedValue))
	      .catch((error) => this._appendMessage('AsyncStorage error: ' + error.message))
	      .done();
	  },
	  _removeStorage() {
	    AsyncStorage.removeItem(STORAGE_KEY)
	      .then(() => this._appendMessage('Selection removed from disk.'))
	      .catch((error) => { this._appendMessage('AsyncStorage error: ' + error.message) })
	      .done();
	  },
	  _appendMessage(message) {
	    this.setState({messages: this.state.messages.concat(message)});
	  },
	});
	exports.title = 'AsyncStorage';
	exports.description = 'Asynchronous local disk storage.';
	exports.examples = [
	  {
	    title: 'Basics - getItem, setItem, removeItem',
	    render(): ReactElement { return <BasicStorageExample />; }
	  },
    ];
```