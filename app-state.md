# iOS 应用程序状态 

`AppStateIOS` 可以告诉你应用程序是在前台还是在后台，而且状态更新时会通知你。
在处理推送通知时，AppStateIOS 经常被用于判断目标和适当的行为。 

## iOS 应用程序状态 

-	Active - 应用程序在前台运行
-	Background - 应用程序在后台运行。用户正在使用另一个应用程序或者在主屏幕上。
-	Inactive - 这是一种过渡状态，目前不会在React Native的应用程序上发生。

想要获取更多的信息，见 [Apple's documentation](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html) 

## 基本用法 

为了查看当前的状态，你可以检查 `AppStateIOS.currentState`，该方法会一直保持最新状态。然而，当 `AppStateIOS` 在桥接器上检索`currentState`时，在启动时它将会为空。

```
    getInitialState: function() {
	  return {
	    currentAppState: AppStateIOS.currentState,
	  };
	},
	componentDidMount: function() {
	  AppStateIOS.addEventListener('change', this._handleAppStateChange);
	},
	componentWillUnmount: function() {
	  AppStateIOS.removeEventListener('change', this._handleAppStateChange);
	},
	_handleAppStateChange: function(currentAppState) {
	  this.setState({ currentAppState, });
	},
	render: function() {
	  return (
 	   <Text>Current state is: {this.state.currentAppState}</Text>
	  );
    },
```

这个例子似乎只能说"当前状态是：活跃的"因为在 `active` 状态时，应用程序只对用户是可见的，空状态只能是暂时的。 

## 方法 

static **addEventListener**(type: string, handler: Function)

通过监听 `change` 事件类型和提供处理程序，为应用程序状态变化添加一个处理程序。

static **removeEventListener**(type: string, handler: Function)

通过传递 `change` 事件类型和处理程序，删除一个处理程序。 

## 例子

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/AppStateIOSExample.js)

```
    'use strict';
	var React = require('react-native');
	var {
	  AppStateIOS,
	  Text,
	View
	} = React;
	var AppStateSubscription = React.createClass({
	  getInitialState() {
	    return {
	      appState: AppStateIOS.currentState,
	      previousAppStates: [],
	    };
	  },
	  componentDidMount: function() {
	    AppStateIOS.addEventListener('change', this._handleAppStateChange);
	  },
	  componentWillUnmount: function() {
	    AppStateIOS.removeEventListener('change', this._handleAppStateChange);
	  },
	  _handleAppStateChange: function(appState) {
	    var previousAppStates = this.state.previousAppStates.slice();
	    previousAppStates.push(this.state.appState);
	    this.setState({
	      appState,
	      previousAppStates,
	    });
	  },
	  render() {
	    if (this.props.showCurrentOnly) {
	      return (
	        <View>
	          <Text>{this.state.appState}</Text>
	        </View>
	      );
	    }
	    return (
	      <View>
	        <Text>{JSON.stringify(this.state.previousAppStates)}</Text>
	      </View>
	    );
	  }
	});
	exports.title = 'AppStateIOS';
	exports.description = 'iOS app background status';
	exports.examples = [
	  {
	    title: 'AppStateIOS.currentState',
	    description: 'Can be null on app initialization',
	    render() { return <Text>{AppStateIOS.currentState}</Text>; }
	  },
	  {
	    title: 'Subscribed AppStateIOS:',
	    description: 'This changes according to the current state, so you can only ever see it rendered as "active"',
	    render(): ReactElement { return <AppStateSubscription showCurrentOnly={true} />; }
	  },
	  {
	    title: 'Previous states:',
	    render(): ReactElement { return <AppStateSubscription showCurrentOnly={false} />; }
	  },
    ];
```