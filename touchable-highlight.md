# 高亮触摸

一个包装器是为了让视图对触发做出合适的响应。按下按钮，包装后的视图的透明性就会降低，这样底衬的颜色就会显示出来，使视图颜色变暗或者着色。底衬的出现是因为向视图层次结构添加了一个视图，如果使用不正确的话，这有时候会导致不必要的认为视觉效果，例如，如果包装了的视图的背景颜色不是很明确的设置成一个不透明的颜色。

例子：

```
renderButton: function() {
  return (
    <TouchableHighlight onPress={this._onPressButton}>
      <Image
        style={styles.button}
        source={require('image!myButton')}
      />
    </TouchableHighlight>
  );
},
```

## 属性 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/Touchable/TouchableHighlight.js) 

[**TouchableWithoutFeedback props...**](http://facebook.github.io/react-native/docs/touchablewithoutfeedback.html#proptypes)

**activeOpacity** 数值型

当触发处于活跃状态时，确定包装后的使徒的不透明度。

**style** [View#style](view.md#style) 

**underlayColor** 字符串型 

当触发处于活跃状态时，底衬的颜色会显示出来。
