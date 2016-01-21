# 计时器 

计时器是一个应用程序的重要的一个组成部分，React Native 实现了[Browser timers](https://developer.mozilla.org/en-US/Add-ons/Code_snippets/Timers)。 

## 计时器

- setTimeout,clearTimeout
- setInterval, clearInterval
- setImmediate, clearImmediate
- requestAnimationFrame, cancelAnimationFrame

`requestAnimationFrame(fn)` 相当于 `setTimeout(fn, 0)`，他们是在刷新屏幕之后被正确触发。

`setImmediate` 是在向本地发送批处理相应之前，当前 JavaScript 执行块结束时执行的。注意，如果你在一个回调函数 `setImmediate` 之内调用 `setImmediate`，它将立即被执行，而且不会返回到本地之间。

这个 `Promise` 的实现是将 `setImmediate` 作为异步性的开端。


## 交互管理器 

良好的原生应用可以用起来感觉很顺利的一个原因是在交互和动画方面避免了复杂的操作。在 React Native，目前我们有一个限制，只有一个JS执行线程，但是你可以使用 `InteractionManager` 来确保在任一交互或者动画完成之后，长期的运行工作的开始是被规划好的。

在下面的交互完成之后，应用程序可以安排任务来运行：

```
    InteractionManager.runAfterInteractions(() => {
		// ...long-running synchronous task...
    });
```

与其他调度方案相比：

- requestAnimationFrame():代码是在时间上的一个动画视图
- setImmediate/setTimeout/setInterval():运行代码之后，请注意这可能会延迟动画
- runAfterInteractions():运行代码之后，没有延迟的动态动画

触发处理系统将一个或多个触发看作是一个“交互”，并且将`runAfterInteractions()` 延迟回调，直到所有的触发都已结束或者被取消。

交互管理器还允许应用程序通过对动画的开始创建一个交互“处理”来注册动画，并且完成之后进行清理：

```
	var handle = InteractionManager.createInteractionHandle();
	// run animation... (`runAfterInteractions` tasks are queued)
	// later, on animation completion:
    InteractionManager.clearInteractionHandle(handle);
    // queued tasks run if all handles were cleared
```
  
## TimerMixin 

我们发现在 React Native 上的应用程序出现致命性问题的主要原因是由于一个组件被卸载后计时器就会被触发。为了解决这个反复出现的问题，我们引入了 `TimerMixin`。如果你有 `TimerMixin`，那么你可以用 `this.setTimeout(fn, 500)`（只是加上 `this.`）来替换 `setTimeout(fn, 500)` 函数的调用，并且当组件被卸载时，一切都会被清理干净。

```
    var TimerMixin = require('react-timer-mixin');
	var Component = React.createClass({
	 mixins: [TimerMixin],
     componentDidMount: function() {
   	   this.setTimeout(
     	 () => { console.log('I do not leak!'); },
    	 500
       ); 
     }
    });
```   

我们强烈建议不用只单独使用 Timers，而是一直使用 mixin，这样将会为你节省很多很难追踪的bugs。