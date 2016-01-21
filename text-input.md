# 文本输入 

通过键盘将文本输入到应用程序的一个基本的组件。属性提供几个功能的可配置性，比如自动校正，自动还原，占位符文本，和不同的键盘类型，如数字键盘。
最简单的一个用例是放置一个 `TextInput`，利用 `onChangeText` 事件来读取用户的输入。还有其他的事件可以使用，比如`onSubmitEditing` 和 `onFocus`。一个简单的例子：

```
<View>
  <TextInput
    style={{height: 40, borderColor: 'gray', borderWidth: 1}}
    onChangeText={(text) => this.setState({input: text})}
  />
  <Text>{'user input: ' + this.state.input}</Text>
</View>
```

`value` 属性可以用于设置输入的值，其目的是让组件的状态更清晰，但是` <TextInput>` 所有的选项都是异步的，所以默认情况下它并不表现的像一个真正的控制组件。就像设置默认值一样设置 `value` 一次，但是你同样可以根据 `onChangeText` 不断的改变它的值。如果你真的想要迫使组件一直都可以恢复到你设置的值，那么你可以设置成 `controlled={true}`。

不是所有版本都支持 `multiline` 属性，然后有些属性只支持多行输入。 

## 属性  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/TextInput/TextInput.js)

**autoCapitalize** enum('none', 'sentences', 'words', 'characters')

可以通知文本输入自动利用某些字符。

-	characters：所有字符，
-	words：每一个单词的首字母
-	sentences：每个句子的首字母（默认情况下）
-	none：不会自动使用任何东西

**autoCorrect** 布尔型

如果值为假，禁用自动校正。默认值为真。
	
**autoFocus** 布尔型

如果值为真，聚焦 componentDidMount 上的文本。默认值为假。

**bufferDelay** 数值型

这个会帮助避免由于 JS 和原生文本输入之间的竞态条件而丢失字符。默认值应该是没问题的，但是如果你每一个按键都操作的非常缓慢，那么你可能想尝试增加这个。

**clearButtonMode** enum('never', 'while-editing', 'unless-editing', 'always')

清除按钮出现在文本视图右侧的时机

**controlled** 布尔型

如果你真想要它表现成一个控制组件，你可以将它的值设置为真，但是按下按键，并且/或者缓慢打字，你可能会看到它闪烁，这取决于你如何处理 onChange 事件。

**editable** 布尔型

如果值为假，文本是不可编辑的。默认值为真。

**enablesReturnKeyAutomatically** 布尔型

如果值为真，当没有文本的时候键盘是不能返回键值的，当有文本的时候会自动返回。默认值为假。

**keyboardType** enum('default', "ascii-capable", 'numbers-and-punctuation', 'url', 'number-pad', 'phone-pad', 'name-phone-pad', 'email-address', 'decimal-pad', 'twitter', 'web-search', "numeric")

决定打开哪种键盘，例如，数字键盘。

**multiline** 布尔型

如果值为真，文本输入可以输入多行。默认值为假。

**onBlur** 函数 

当文本输入是模糊的，调用回调函数

**onChange** 函数

当文本输入的文本发生变化时，调用回调函数

**onChangeText** 函数

**onEndEditing** 函数 

**onFocus** 函数

当输入的文本是聚焦状态时，调用回调函数

**onSubmitEditing** 函数

**password** 布尔型

如果值为真，文本输入框就成为一个密码区域。默认值为假。

**placeholder** 字符串型

在文本输入之前字符串将被呈现出来

**placeholderTextColor** 字符串型

占位符字符串的文本颜色

**returnKeyType** enum('default', 'go', 'google', 'join', 'next', 'route', 'search', 'send', 'yahoo', 'done', 'emergency-call')

决定返回键的样式

**secureTextEntry** 布尔型

如果值为真，文本输入框就会使输入的文本变得模糊，以便于像密码这样敏感的文本保持安全。默认值为假。

**selectionState** 文档选择状态

见 DocumentSelectionState.js，一些状态是为了维护一个文档的选择信息。

**style** [Text#style](text.md#style) 

**testID** 字符串型

用于端对端测试时定位视图。

**value** 字符串型

文本输入的默认值

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/TextInputExample.js)

```
'use strict';
var React = require('react-native');
var {
  Text,
  TextInput,
  View,
  StyleSheet,
} = React;
var WithLabel = React.createClass({
  render: function() {
    return (
      <View style={styles.labelContainer}>
        <View style={styles.label}>
          <Text>{this.props.label}</Text>
        </View>
        {this.props.children}
      </View>
    );
  }
});
var TextEventsExample = React.createClass({
  getInitialState: function() {
    return {
      curText: '<No Event>',
      prevText: '<No Event>',
    };
  },
  updateText: function(text) {
    this.setState({
      curText: text,
      prevText: this.state.curText,
    });
  },
  render: function() {
    return (
      <View>
        <TextInput
          autoCapitalize="none"
          placeholder="Enter text to see events"
          autoCorrect={false}
          onFocus={() => this.updateText('onFocus')}
          onBlur={() => this.updateText('onBlur')}
          onChange={(event) => this.updateText(
            'onChange text: ' + event.nativeEvent.text
          )}
          onEndEditing={(event) => this.updateText(
            'onEndEditing text: ' + event.nativeEvent.text
          )}
          onSubmitEditing={(event) => this.updateText(
            'onSubmitEditing text: ' + event.nativeEvent.text
          )}
          style={styles.default}
        />
        <Text style={styles.eventLabel}>
          {this.state.curText}{'\n'}
          (prev: {this.state.prevText})
        </Text>
      </View>
    );
  }
});
var styles = StyleSheet.create({
  page: {
    paddingBottom: 300,
  },
  default: {
    height: 26,
    borderWidth: 0.5,
    borderColor: '#0f0f0f',
    padding: 4,
    flex: 1,
    fontSize: 13,
  },
  multiline: {
    borderWidth: 0.5,
    borderColor: '#0f0f0f',
    flex: 1,
    fontSize: 13,
    height: 50,
  },
  eventLabel: {
    margin: 3,
    fontSize: 12,
  },
  labelContainer: {
    flexDirection: 'row',
    marginVertical: 2,
    flex: 1,
  },
  label: {
    width: 80,
    justifyContent: 'flex-end',
    flexDirection: 'row',
    marginRight: 10,
    paddingTop: 2,
  },
});
exports.title = '<TextInput>';
exports.description = 'Single-line text inputs.';
exports.examples = [
  {
    title: 'Auto-focus',
    render: function() {
      return <TextInput autoFocus={true} style={styles.default} />;
    }
  },
  {
    title: 'Auto-capitalize',
    render: function() {
      return (
        <View>
          <WithLabel label="none">
            <TextInput
              autoCapitalize="none"
              style={styles.default}
            />
          </WithLabel>
          <WithLabel label="sentences">
            <TextInput
              autoCapitalize="sentences"
              style={styles.default}
            />
          </WithLabel>
          <WithLabel label="words">
            <TextInput
              autoCapitalize="words"
              style={styles.default}
            />
          </WithLabel>
          <WithLabel label="characters">
            <TextInput
              autoCapitalize="characters"
              style={styles.default}
            />
          </WithLabel>
        </View>
      );
    }
  },
  {
    title: 'Auto-correct',
    render: function() {
      return (
        <View>
          <WithLabel label="true">
            <TextInput autoCorrect={true} style={styles.default} />
          </WithLabel>
          <WithLabel label="false">
            <TextInput autoCorrect={false} style={styles.default} />
          </WithLabel>
        </View>
      );
    }
  },
  {
    title: 'Keyboard types',
    render: function() {
      var keyboardTypes = [
        'default',
        'ascii-capable',
        'numbers-and-punctuation',
        'url',
        'number-pad',
        'phone-pad',
        'name-phone-pad',
        'email-address',
        'decimal-pad',
        'twitter',
        'web-search',
        'numeric',
      ];
      var examples = keyboardTypes.map((type) => {
        return (
          <WithLabel key={type} label={type}>
            <TextInput
              keyboardType={type}
              style={styles.default}
            />
          </WithLabel>
        );
      });
      return <View>{examples}</View>;
    }
  },
  {
    title: 'Return key types',
    render: function() {
      var returnKeyTypes = [
        'default',
        'go',
        'google',
        'join',
        'next',
        'route',
        'search',
        'send',
        'yahoo',
        'done',
        'emergency-call',
      ];
      var examples = returnKeyTypes.map((type) => {
        return (
          <WithLabel key={type} label={type}>
            <TextInput
              returnKeyType={type}
              style={styles.default}
            />
          </WithLabel>
        );
      });
      return <View>{examples}</View>;
    }
  },
  {
    title: 'Enable return key automatically',
    render: function() {
      return (
        <View>
          <WithLabel label="true">
            <TextInput enablesReturnKeyAutomatically={true} style={styles.default} />
          </WithLabel>
        </View>
      );
    }
  },
  {
    title: 'Secure text entry',
    render: function() {
      return (
        <View>
          <WithLabel label="true">
            <TextInput secureTextEntry={true} style={styles.default} value="abc" />
          </WithLabel>
        </View>
      );
    }
  },
  {
    title: 'Event handling',
    render: function(): ReactElement { return <TextEventsExample /> },
  },
  {
    title: 'Colored input text',
    render: function() {
      return (
        <View>
          <TextInput
            style={[styles.default, {color: 'blue'}]}
            value="Blue"
          />
          <TextInput
            style={[styles.default, {color: 'green'}]}
            value="Green"
          />
        </View>
      );
    }
  },
  {
    title: 'Clear button mode',
    render: function () {
      return (
        <View>
          <WithLabel label="never">
            <TextInput
              style={styles.default}
              clearButtonMode="never"
            />
          </WithLabel>
          <WithLabel label="while editing">
            <TextInput
              style={styles.default}
              clearButtonMode="while-editing"
            />
          </WithLabel>
          <WithLabel label="unless editing">
            <TextInput
              style={styles.default}
              clearButtonMode="unless-editing"
            />
          </WithLabel>
          <WithLabel label="always">
            <TextInput
              style={styles.default}
              clearButtonMode="always"
            />
          </WithLabel>
        </View>
      );
    }
  },
];
```