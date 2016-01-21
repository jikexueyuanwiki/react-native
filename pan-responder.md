# 全景响应器 

`PanResponder` 将几个触发调节成一个单一的触发动作。该方法可以使单一触发动作对额外的触发具有弹性，可以用来识别简单的多点触发动作。

它为响应处理程序提供了一个可预测包，这个相应处理程序是由[动作应答系统](http://facebook.github.io/react-native/docs/gesture-responder-system.html)提供的。对每一个处理程序，在正常事件旁提供了一个新的 `gestureState` 对象。
一个 `gestureState` 对象有以下属性：

-	`stateID`-gestureState 的ID-在屏幕上保持至少一个触发动作的时间
-	`moveX`-最近动态触发的最新的屏幕坐标
-	`x0`-应答器横向的屏幕坐标
-	`y0`-应答器纵向的屏幕坐标
-	`dx`-触发开始后累积的横向动作距离
-	`dy`-触发开始后累积的纵向动作距离
-	`vx`-当前手势的横向速度
-	`vy`-当前手势的纵向速度
-	`numberActiveTouch`-屏幕上当前触发的数量 
	
## 基本用法 


```
    componentWillMount: function() {
	    this._panGesture = PanResponder.create({
	      // Ask to be the responder:
	      onStartShouldSetPanResponder: (evt, gestureState) => true,
	      onStartShouldSetPanResponderCapture: (evt, gestureState) => true,
	      onMoveShouldSetPanResponder: (evt, gestureState) => true,
	      onMoveShouldSetPanResponderCapture: (evt, gestureState) => true,
	      onPanResponderGrant: (evt, gestureState) => {
	        // The guesture has started. Show visual feedback so the user knows
	        // what is happening!
	        // gestureState.{x,y}0 will be set to zero now
	      },
	      onPanResponderMove: (evt, gestureState) => {
	        // The most recent move distance is gestureState.move{X,Y}
	        // The accumulated gesture distance since becoming responder is
 	       // gestureState.d{x,y}
	      },
	      onResponderTerminationRequest: (evt, gestureState) => true,
	      onPanResponderRelease: (evt, gestureState) => {
	        // The user has released all touches while this view is the
	        // responder. This typically means a gesture has succeeded
	      },
	      onPanResponderTerminate: (evt, gestureState) => {
	        // Another component has become the responder, so this gesture
	        // should be cancelled
	      },
	    });
	  },
	  render: function() {
	    return (
 	     <View {...this._panResponder.panHandlers} />
	    );
      },
```

## 工程实例

想要查看它的实际应用，尝试 [PanResponder example in UIExplorer](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/ResponderExample.js)。

## 方法 

static **create**(config: object) 

@param {object} 所有应答器回调配置的增强版本，应答器回调不仅可以提供典型的 `ResponderSyntheticEvent`，还可以提供 `PanResponder` 动作状态。在每一个典型的 `onResponder*` 回调中，用 `PanResponder` 对 `Responder` 做简单的替换。例如， `config`对象可能看起来像如下形式：

- `onMoveShouldSetPanResponder: (e, gestureState) => {...}`
- `onMoveShouldSetPanResponderCapture: (e, gestureState) => {...}`
- `onStartShouldSetPanResponder: (e, gestureState) => {...}`
- `onStartShouldSetPanResponderCapture: (e, gestureState) => {...}`
- `onPanResponderReject: (e, gestureState) => {...}`
- `onPanResponderGrant: (e, gestureState) => {...}`
- `onPanResponderStart: (e, gestureState) => {...}`
- `onPanResponderEnd: (e, gestureState) => {...}`
- `onPanResponderRelease: (e, gestureState) => {...}`
- `onPanResponderMove: (e, gestureState) => {...}`
- `onPanResponderTerminate: (e, gestureState) => {...}`
- `onPanResponderTerminationRequest: (e, gestureState) => {...}`

一般来说，对于那些捕获的等价事件，我们在捕获阶段更新一次  gestureState ，并且也可以在冒泡阶段使用。

在 onStartShould* 回调时需要注意一点。在对节点的捕获/冒泡阶段的开始/结束事件中，它们只对更新后的 `gestureState` 做出反应。一旦节点成为应答器，你可以依靠每一个被动作和 `gestureState` 处理后相应更新的开始/结束事件。 (numberActiveTouches) 可能不完全准确，除非你是应答器。


