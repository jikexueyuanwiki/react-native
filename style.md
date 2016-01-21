# 样式 

React Native 不实现 CSS，而是依赖于 JavaScript 来为你的应用程序设置样式。这是一个有争议的决定，你可以阅读那些幻灯片，了解背后的基本原理。

## 声明样式

在 React Native 中声明样式的方法如下：


```
var styles = StyleSheet.create({
  base: {
    width: 38,
    height: 38,
  },
  background: {
    backgroundColor: '#222222',
  },
  active: {
    borderWidth: 2,
    borderColor: '#00ff00',
  },
}); 
```

`StyleSheet.create` 的创建是可选的，但提供了一些关键优势。它通过将它们转换为引用一个内部表的纯数字，来确保值是**不可变的**和**不透明的**。通过将它放在文件的最后，也确保了它们为应用程序只创建一次，而不是每一个 render 都创建。

所有的属性名称和值是工作在网络中的一个子集。对于布局来说，React Native实现了 [Flexbox](flexbox.md)。

## 使用样式

所有的核心组件接受样式属性。


``` 
<Text style={styles.base} />
<View style={styles.background} />
```

它们也接受一系列的样式。

```
<View style={[styles.base, styles.background]} />
```

行为与 `Object.assign` 相同：在冲突值的情况下，从最右边元素的值将会优先，并且 falsy 值如 `false`，`undefined` 和 `null` 将被忽略。一个常见的模式是基于某些条件有条件地添加一个样式。


```
<View style={[styles.base, this.state.active && styles.active]} /> 
```

最后，如果真的需要，您还可以在render中创建样式对象，但是这种做法非常不赞成。最后把它们放在数组定义中。

``` 
<View
  style={[styles.base, {
    width: this.state.width,
    height: this.state.width * this.state.aspectRatio
  }]}
/>
```

## 样式传递

为了让一个 call site 定制你的子组件的样式，你可以通过样式传递。使用 `View.propTypes.style` 和 `Text.propTypes.style`，以确保只有样式被传递了。


```
var List = React.createClass({
  propTypes: {
    style: View.propTypes.style,
    elementStyle: View.propTypes.style,
  },
  render: function() {
    return (
      <View style={this.props.style}>
        {elements.map((element) =>
          <View style={[styles.element, this.props.elementStyle]} />
        )}
      </View>
    );
  }
});
// ... in another file ...
<List style={styles.list} elementStyle={styles.listElement} />
```

## 属性支持

你可以在以下的链接中检测最新的 CSS 属性支持。 

- [View 属性](view.md#style)
- [Image 属性](image.md#style)
- [Text 属性](text.md#style)
- [Flex 属性](flexbox.md)
