# 文本 

用于显示文本的响应组件，支持嵌套、样式和触发处理。在接下来的例子中，嵌套的标题和正文文本将从 `styles.baseText` 继承 `FontFamily`，但是标题会提供它自己其他的设计风格。标题和正文在文字换行时会堆叠在彼此之上。

``` 
renderText: function() {
  return (
    <Text style={styles.baseText}>
      <Text style={styles.titleText} onPress={this.onPressTitle}>
        {this.state.titleText + '\n\n'}
      </Text>
      <Text numberOfLines={5}>
        {this.state.bodyText}
      </Text>
    </Text>
  );
},
...
var styles = StyleSheet.create({
  baseText: {
    fontFamily: 'Cochin',
  },
  titleText: {
    fontSize: 20,
    fontWeight: 'bold',
  },
};
```

### Props  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Text/Text.js) 

**numberOfLines** 数值型

用于在计算文本布局后将文本和省略符号进行截断，包括换行，这样总的行数就不会超过这个值。

**onPress** 函数

这个函数被称为按下。在默认高亮状态下，文本内部是支持按下动作处理的（该函数在 `suppressHighlighting` 是禁用的）。

<a name="style"></a>
**style** style

[**View#style...**](view.md#style)

**color** 字符串型

**containerBackgroundColor** 字符串型

**fontFamily** 字符串型

**fontSize** 数值型

**fontStyle** enum('normal', 'italic')

**fontWeight** enum("normal", 'bold', '100', '200', '300', '400', '500', '600', '700', '800', '900')

**lineHeight** 数值型

**textAlign** enum("auto", 'left', 'right', 'center')

**writingDirection** enum("auto", 'ltr', 'rtl')

**suppressHighlighting** 布尔型

值为真时，当文本被按下时没有视觉上的变化。默认情况下，按下之前是一个灰色椭圆高亮的文本。

**testID** 字符串型

在端到端测试时用于定位视图 

### 描述 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/docs/Text.md)

## 嵌套文本

在 iOS 里，显示格式化文本的方式是使用 `NSAttributedString` ：你可以为你想要显示和注释的文本划定一些特定的格式范围。实际上，这是非常无聊的。对于 React Native，我们决定使用 Web 模式，在这里我们可以利用嵌套文本来达到同样的效果。

```
    <Text style={{fontWeight: 'bold'}}>
	  I am bold
	  <Text style={{color: 'red'}}>
	    and red
	  </Text>
	</Text>
```

在幕后，这将会被转换成一个完全的包含以下信息的 `NSAttributedString`

```
    "I am bold and red"
	0-9: bold
	9-17: bold, red
```

## 容器

`<Text>` 元素是与布局设计有特定关系的：内部的一切都不再使用 flexbox 布局而是使用文本布局。这意味着一个 `<Text>` 内部的元素不在是矩形的，而是在结尾的时候被包装起来。

```
    <Text>
	  <Text>First part and </Text>
	  <Text>second part</Text>
	</Text>
	// Text container: all the text flows as if it was one
	// |First part |
	// |and second |
	// |part       |
	<View>
	  <Text>First part and </Text>
	  <Text>second part</Text>
	</View>
	// View container: each text is its own block
	// |First part |
	// |and        |
	// |second part|
```

## 有限制性的样式继承

在网络上，为整个文档设置字体体系和大小的常用方法是：

```
    /* CSS, *not* React Native */
	html {
	  font-family: 'lucida grande', tahoma, verdana, arial, sans-serif;
	  font-size: 11px;
	  color: #141823;
    }
```

当浏览器想要显示一个文本节点时，它会一直走到树的根元素，并且找到一个有 `font-size`属性的元素。该系统一个意想不到的性质是**任何**节点都可以有`font-size`属性，包括一个 `<div>`。这是为了方便而设计的，尽管语义并不是正确的。

在 React Naitve 里，我们关于这一点会更严格：**你必须将 `<Text>` 组件里的所有节点都进行包装**；你不能在 `<View>`下直接拥有一个文本节点。

```
    // BAD: will fatal, can't have a text node as child of a <View>
	<View>
	  Some text
	</View>
	// GOOD
	<View>
	  <Text>
	    Some text
	   </Text>
    </View>
```

你也失去了对整个子树设置字体的能力。为了在你的应用程序里使用一致为字体和大小，推荐使用的方法是创建一个包括他们的 `MyAppText` 组件，并且在你的应用程序里使用这个组件。你可以使用该组件来构成更多特定的组件，比如用于其他类型文本的 `MyAppHeaderText` 组件。

```
    <View>
	  <MyAppText>Text styled with the default font for the entire application</MyAppText>
	  <MyAppHeaderText>Text styled as a header</MyAppHeaderText>
    </View>
```

React Native 还有继承风格的概念，但是仅限于文本子树。在这种情况下，第二部分将是粗体和红色。

```
    <Text style={{fontWeight: 'bold'}}>
	  I am bold
	  <Text style={{color: 'red'}}>
	    and red
	  </Text>
    </Text>
```

我们相信更多的文本约束方法将会产生更好的应用程序：

-	（开发人员）响应组件的设计源于大脑中孤立的想法：你应该有能力将你的组件放置在你应用程序的任何一个地方，相信只有工具是相同的，那么它的表现和行为都是相同的。文本属性是可以从工具外继承的，这会打破这种孤立。
-	（实现人员）React Native 实现也是很简单的。我们不需要对每一个单一的元素都要有一个 `FontFamily` 模块，并且我们在每一次显示一个文本节点时也不需要对树遍历到根节点。风格的继承只需要在原生文本内部进行编码，不需要泄露给其他文本或者是系统本身。

### 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/TextExample.ios.js)

``` 
'use strict';
var React = require('react-native');
var {
  StyleSheet,
  Text,
  View,
} = React;
var Entity = React.createClass({
  render: function() {
    return (
      <Text style={styles.entity}>
        {this.props.children}
      </Text>
    );
  }
});
var AttributeToggler = React.createClass({
  getInitialState: function() {
    return {fontWeight: '500', fontSize: 15};
  },
  increaseSize: function() {
    this.setState({
      fontSize: this.state.fontSize + 1
    });
  },
  render: function() {
    var curStyle = {fontSize: this.state.fontSize};
    return (
      <Text>
        <Text style={curStyle}>
          Tap the controls below to change attributes.
        </Text>
        <Text>
          See how it will even work on{' '}
          <Text style={curStyle}>
            this nested text
          </Text>
          <Text onPress={this.increaseSize}>
            {'>> Increase Size <<'}
          </Text>
        </Text>
      </Text>
    );
  }
});
exports.title = '<Text>';
exports.description = 'Base component for rendering styled text.';
exports.displayName = 'TextExample';
exports.examples = [
{
  title: 'Wrap',
  render: function() {
    return (
      <Text>
        The text should wrap if it goes on multiple lines. See, this is going to
        the next line.
      </Text>
    );
  },
}, {
  title: 'Padding',
  render: function() {
    return (
      <Text style={{padding: 10}}>
        This text is indented by 10px padding on all sides.
      </Text>
    );
  },
}, {
  title: 'Font Family',
  render: function() {
    return (
      <View>
        <Text style={{fontFamily: 'Cochin'}}>
          Cochin
        </Text>
        <Text style={{fontFamily: 'Cochin', fontWeight: 'bold'}}>
          Cochin bold
        </Text>
        <Text style={{fontFamily: 'Helvetica'}}>
          Helvetica
        </Text>
        <Text style={{fontFamily: 'Helvetica', fontWeight: 'bold'}}>
          Helvetica bold
        </Text>
        <Text style={{fontFamily: 'Verdana'}}>
          Verdana
        </Text>
        <Text style={{fontFamily: 'Verdana', fontWeight: 'bold'}}>
          Verdana bold
        </Text>
      </View>
    );
  },
}, {
  title: 'Font Size',
  render: function() {
    return (
      <View>
        <Text style={{fontSize: 23}}>
          Size 23
        </Text>
        <Text style={{fontSize: 8}}>
          Size 8
        </Text>
      </View>
    );
  },
}, {
  title: 'Color',
  render: function() {
    return (
      <View>
        <Text style={{color: 'red'}}>
          Red color
        </Text>
        <Text style={{color: 'blue'}}>
          Blue color
        </Text>
      </View>
    );
  },
}, {
  title: 'Font Weight',
  render: function() {
    return (
      <View>
        <Text style={{fontWeight: '100'}}>
          Move fast and be ultralight
        </Text>
        <Text style={{fontWeight: '200'}}>
          Move fast and be light
        </Text>
        <Text style={{fontWeight: 'normal'}}>
          Move fast and be normal
        </Text>
        <Text style={{fontWeight: 'bold'}}>
          Move fast and be bold
        </Text>
        <Text style={{fontWeight: '900'}}>
          Move fast and be ultrabold
        </Text>
      </View>
    );
  },
},  {
  title: 'Font Style',
  render: function() {
    return (
      <View>
        <Text style={{fontStyle: 'normal'}}>
          Normal text
        </Text>
        <Text style={{fontStyle: 'italic'}}>
          Italic text
        </Text>
      </View>
    );
  },
}, {
  title: 'Nested',
  description: 'Nested text components will inherit the styles of their ' +
    'parents (only backgroundColor is inherited from non-Text parents).  ' +
    '<Text> only supports other <Text> and raw text (strings) as children.',
  render: function() {
    return (
      <View>
        <Text>
          (Normal text,
          <Text style={{fontWeight: 'bold'}}>
            (and bold
            <Text style={{fontSize: 11, color: '#527fe4'}}>
              (and tiny inherited bold blue)
            </Text>
            )
          </Text>
          )
        </Text>
        <Text style={{fontSize: 12}}>
          <Entity>Entity Name</Entity>
        </Text>
      </View>
    );
  },
}, {
  title: 'Text Align',
  render: function() {
    return (
      <View>
        <Text style={{textAlign: 'left'}}>
          left left left left left left left left left left left left left left left
        </Text>
        <Text style={{textAlign: 'center'}}>
          center center center center center center center center center center center
        </Text>
        <Text style={{textAlign: 'right'}}>
          right right right right right right right right right right right right right
        </Text>
      </View>
    );
  },
}, {
  title: 'Spaces',
  render: function() {
    return (
      <Text>
        A {'generated'} {' '} {'string'} and    some &nbsp;&nbsp;&nbsp; spaces
      </Text>
    );
  },
}, {
  title: 'Line Height',
  render: function() {
    return (
      <Text>
        <Text style={{lineHeight: 35}}>
          A lot of space between the lines of this long passage that should
          wrap once.
        </Text>
      </Text>
    );
  },
}, {
  title: 'Empty Text',
  description: 'It\'s ok to have Text with zero or null children.',
  render: function() {
    return (
      <Text />
    );
  },
}, {
  title: 'Toggling Attributes',
  render: function(): ReactElement {
    return <AttributeToggler />;
  },
}, {
  title: 'backgroundColor attribute',
  description: 'backgroundColor is inherited from all types of views.',
  render: function() {
    return (
      <View style={{backgroundColor: 'yellow'}}>
        <Text>
          Yellow background inherited from View parent,
          <Text style={{backgroundColor: '#ffaaaa'}}>
            {' '}red background,
            <Text style={{backgroundColor: '#aaaaff'}}>
              {' '}blue background,
              <Text>
                {' '}inherited blue background,
                <Text style={{backgroundColor: '#aaffaa'}}>
                  {' '}nested green background.
                </Text>
              </Text>
            </Text>
          </Text>
        </Text>
      </View>
    );
  },
}, {
  title: 'containerBackgroundColor attribute',
  render: function() {
    return (
      <View>
        <View style={{flexDirection: 'row', height: 85}}>
          <View style={{backgroundColor: '#ffaaaa', width: 150}} />
          <View style={{backgroundColor: '#aaaaff', width: 150}} />
        </View>
        <Text style={[styles.backgroundColorText, {top: -80}]}>
          Default containerBackgroundColor (inherited) + backgroundColor wash
        </Text>
        <Text style={[
          styles.backgroundColorText,
          {top: -70, containerBackgroundColor: 'transparent'}]}>
          {"containerBackgroundColor: 'transparent' + backgroundColor wash"}
        </Text>
      </View>
    );
  },
}, {
  title: 'numberOfLines attribute',
  render: function() {
    return (
      <View>
        <Text numberOfLines={1}>
          Maximum of one line no matter now much I write here. If I keep writing it{"'"}ll just truncate after one line
        </Text>
        <Text numberOfLines={2} style={{marginTop: 20}}>
          Maximum of two lines no matter now much I write here. If I keep writing it{"'"}ll just truncate after two lines
        </Text>
        <Text style={{marginTop: 20}}>
          No maximum lines specified no matter now much I write here. If I keep writing it{"'"}ll just keep going and going
        </Text>
      </View>
    );
  },
}];
var styles = StyleSheet.create({
  backgroundColorText: {
    left: 5,
    backgroundColor: 'rgba(100, 100, 100, 0.3)'
  },
  entity: {
    fontWeight: '500',
    color: '#527fe4',
  },
});
```
