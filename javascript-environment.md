# JavaScript 环境 

## JavaScript 运行时间

当使用 React Native 时，你将会在两个环境中运行 JavaScript 代码：

- 在模拟器和电话中：[JavaScriptCore]( http://trac.webkit.org/wiki/JavaScriptCore) 是 JavaScript 的引擎，能够驱动 Safari 和 web 视图。由于在 iOS 应用程序中没有可写的可执行的内存，它不用 JIT 运行。
- 使用 Chrome 调试时，它在 Chrome 本身中运行所有 JavaScript 代码，并且通过 WebSocket 与 Objective-C 交互。所以你正在使用 [V8]( https://code.google.com/p/v8/)。

虽然两个环境很相似，但是你可能会以触及一些矛盾而结束。将来我们很可能去尝试其他 JS 引擎，所以最好避免依赖任何运行时的细节。

## JavaScript 转换

React Native 附带许多 JavaScript 转换，使编写代码更愉快。如果你好奇的话，你可以查看[所有这些转换的实现]( https://github.com/facebook/jstransform/tree/master/visitors)。这是完整的列表：

ES5

- 关键字：`promise.catch(function() { });`

ES6

- 箭头函数：`<C onPress={() => this.setState({pressed: true})}`
- 调用传播：`Math.max(...array);`
- 类：`class C extends React.Component { render() { return <View />; } }`
- 解构：`var {isActive, style} = this.props;`
- 迭代：`for (var element of array) { }`
- 计算属性：`var key = 'abc'; var obj = {[key]: 10};`
- 对象 Consise 方法：`var obj = { method() { return 10; } };`
- 对象 short 表示法：`var name = 'vjeux'; var obj = { name };`
- 其他参数：`function(type, ...args) { }`
- 模板： `var who = 'world'; var str = 'Hello ${who}';`

ES7

- 对象传播：`var extended = { ...obj, a: 10 };`
- Trailing Comma 函数：`function f(a, b, c,) { }`
