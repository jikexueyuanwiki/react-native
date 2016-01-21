# ProgressBarAndroid

React 组建包裹了只是 Android 部分的 `ProgressBar`。这个组件是被用来提示这个应用正在加载或者在应用里面有一些操作。

例子：

```javascript
render: function() {
  var progressBar =
    <View style={styles.container}>
      <ProgressBar styleAttr="Inverse" />
    </View>;

  return (
    <MyLoadingComponent
      componentView={componentView}
      loadingView={progressBar}
      style={styles.loadingComponent}
    />
  );
},
```   

## 属性

**styleAttr** 样式属性 

进度条的样式，包括：

- Horizontal
- Small
- Large
- Inverse
- SmallInverse
- LargeInverse

**testID** 字符串 
在端对端测试里面被用来定位这个视图。

## 例子

```javascript
'use strict';

var ProgressBar = require('ProgressBarAndroid');
var React = require('React');
var UIExplorerBlock = require('UIExplorerBlock');
var UIExplorerPage = require('UIExplorerPage');

var ProgressBarAndroidExample = React.createClass({

  statics: {
    title: '<ProgressBarAndroid>',
    description: 'Visual indicator of progress of some operation. ' +
        'Shows either a cyclic animation or a horizontal bar.',
  },

  render: function() {
    return (
      <UIExplorerPage title="ProgressBar Examples">
        <UIExplorerBlock title="Default ProgressBar">
          <ProgressBar />
        </UIExplorerBlock>

        <UIExplorerBlock title="Small ProgressBar">
          <ProgressBar styleAttr="Small" />
        </UIExplorerBlock>

        <UIExplorerBlock title="Large ProgressBar">
          <ProgressBar styleAttr="Large" />
        </UIExplorerBlock>

        <UIExplorerBlock title="Inverse ProgressBar">
          <ProgressBar styleAttr="Inverse" />
        </UIExplorerBlock>

        <UIExplorerBlock title="Small Inverse ProgressBar">
          <ProgressBar styleAttr="SmallInverse" />
        </UIExplorerBlock>

        <UIExplorerBlock title="Large Inverse ProgressBar">
          <ProgressBar styleAttr="LargeInverse" />
        </UIExplorerBlock>
      </UIExplorerPage>
    );
  },
});

module.exports = ProgressBarAndroidExample;
```
