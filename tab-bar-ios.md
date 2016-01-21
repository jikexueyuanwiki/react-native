# TabBarIOS.Item

## 工具   [Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/TabBarIOS/TabBarItemIOS.ios.js)

**badge** 并

位于图标右上角的小的红色的泡沫。

**icon** Image.propTypes.source

标记的自动以图标。当定义了系统图标时，它将被忽略。

**onPress** 函数

当标记被选中时，该函数回调，你应该改变组件的状态来设置 selected={true}。

**selected** 布尔值

它指定了孩子是否可见。如果你看到了一个空白的内容，你很有可能是忘记添加选中项了。

**selectedIcon** Image.propTypes.source

当标记被选中时，自定义的图标。当定义了系统图标时，它会被忽略。如果为空，那么图标会呈现蓝色。

**style** [View#style](http://facebook.github.io/react-native/docs/view.html#style)

React 样式对象。

**systemIcon** 枚举（'bookmarks'，'contacts'，'downloads'，'favorites'，'featured'，'history'，'more'，'most-recent'，'most-viewed'，'recents'，'search'，'top-rated'）

项目有一些预定义的系统图标。请注意如果你正在使用它们，标题和选中的图标将被系统图标覆盖。

**title** 字符串

出现在图标下的文本。当定义了系统图标时，它会被忽略。