# 不透明触摸

一个包装器是为了让视图对触发做出合适的响应。按下按钮，包装后的视图的透明性就会降低，变暗。这个动作的完成实际上并没有改变视图的层次，一般来说很容易添加到一个应用程序，并且不会产生奇怪的副作用。

例子：

``` 
renderButton: function() {
  return (
    <TouchableOpacity onPress={this._onPressButton}>
      <Image
        style={styles.button}
        source={require('image!myButton')}
      />
    </TouchableOpacity>
  );
},
```

## 工具 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/Touchable/TouchableOpacity.js) 

[TouchableWithoutFeedback props...](http://facebook.github.io/react-native/docs/touchablewithoutfeedback.html#proptypes) 

**activeOpacity** 数值 

当触发处于活跃状态时，确定包装后的使徒的不透明度。
