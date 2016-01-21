# 交互管理器 

交互管理器在任意交互/动画完成之后，允许安排长期的运行工作。特别是，这允许 JavaScript 动画可以顺利的运行。

应用程序可以在交互完成之后根据以下代码来安排运行任务：

```
    InteractionManager.runAfterInteractions(() => {
	  // ...long-running synchronous task...
    });
```

与其他调度方案进行比较：

-	requestAnimationFrame():代码是动画在时间上的一个视图
-	setImmediate/setTimeout():运行代码后，请注意这有可能会延迟动画
-	runAfterInteractions(): 运行代码后，没有延迟的动态动画
	
触发处理系统将一个或者多个动态触发看成是一个“交互”，并且将推迟 `runAfterInteractions()` 回调直到所有的触发都已经结束或者被取消了。

交互管理器还允许应用程序通过创建一个“处理”动画的开端来注册动画，结束之后进行清除：

```
    var handle = InteractionManager.createInteractionHandle();
	// run animation... (`runAfterInteractions` tasks are queued)
	// later, on animation completion:
	InteractionManager.clearInteractionHandle(handle);
    // queued tasks run if all handles were cleared
```

## 方法 ##
static **runAfterInteractions**(callback: Function)

在所有交互都完成之后安排一个函数来运行。

static **createInteractionHandle**()

通知管理器已经启动了一个交互。

static **clearInteractionHandle**(handle: number) 

通知管理器一个交互动作已经完成了。


