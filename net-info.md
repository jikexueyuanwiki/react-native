# 网络信息 

网络信息公开在线或者离线信息 

## reachabilityIOS 

异步确定设备是否处于在线状态并且在蜂窝网络。

-	None - 设备处于离线状态
-	WiFi - 设备处于在线状态，并且通过 WiFi 或者是 iOS 模拟器连接
-	Cell - 设备通过网络连接，3G，WiMax，或者 LTE 进行连接
-	Unknown - 错误情况，并且网络状态未知

```
    NetInfo.reachabilityIOS.fetch().done((reach) => {
	  console.log('Initial: ' + reach);
	});
	function handleFirstReachabilityChange(reach) {
	  console.log('First change: ' + reach);
	  NetInfo.reachabilityIOS.removeEventListener(
	    'change',
	    handleFirstReachabilityChange
	  );
	}
	NetInfo.reachabilityIOS.addEventListener(
	  'change',
	  handleFirstReachabilityChange
    );
```

## 连接状态

在所有的平台上都可用。异步获取一个布尔值来确定网络连接。

```
    NetInfo.isConnected.fetch().done((isConnected) => {
	  console.log('First, is ' + (isConnected ? 'online' : 'offline'));
	});
	function handleFirstConnectivityChange(isConnected) {
	  console.log('Then, is ' + (isConnected ? 'online' : 'offline'));
	  NetInfo.isConnected.removeEventListener(
	    'change',
	    handleFirstConnectivityChange
	  );
	}
	NetInfo.isConnected.addEventListener(
	  'change',
	  handleFirstConnectivityChange
    );
```

## 例子

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/NetInfoExample.js)

```
    'use strict';
	var React = require('react-native');
	var {
	  NetInfo,
	  Text,
	  View
	} = React;
	var ReachabilitySubscription = React.createClass({
	  getInitialState() {
	    return {
	      reachabilityHistory: [],
	    };
	  },
	  componentDidMount: function() {
	    NetInfo.reachabilityIOS.addEventListener(
	      'change',
	      this._handleReachabilityChange
	    );
	  },
	  componentWillUnmount: function() {
	    NetInfo.reachabilityIOS.removeEventListener(
	      'change',
	      this._handleReachabilityChange
	    );
	  },
	  _handleReachabilityChange: function(reachability) {
	    var reachabilityHistory = this.state.reachabilityHistory.slice();
	    reachabilityHistory.push(reachability);
	    this.setState({
	      reachabilityHistory,
	    });
	  },
	  render() {
	    return (
	      <View>
	        <Text>{JSON.stringify(this.state.reachabilityHistory)}</Text>
	      </View>
	    );
	  }
	});
	var ReachabilityCurrent = React.createClass({
	  getInitialState() {
	    return {
	      reachability: null,
	    };
	  },
	  componentDidMount: function() {
	    NetInfo.reachabilityIOS.addEventListener(
	      'change',
	      this._handleReachabilityChange
	    );
	    NetInfo.reachabilityIOS.fetch().done(
	      (reachability) => { this.setState({reachability}); }
	    );
	  },
	  componentWillUnmount: function() {
	    NetInfo.reachabilityIOS.removeEventListener(
	      'change',
	      this._handleReachabilityChange
	    );
	  },
	  _handleReachabilityChange: function(reachability) {
	    this.setState({
	      reachability,
 	   });
	  },
	  render() {
	    return (
	      <View>
	        <Text>{this.state.reachability}</Text>
	      </View>
	    );
	  }
	});
	var IsConnected = React.createClass({
	  getInitialState() {
	    return {
	      isConnected: null,
	    };
	  },
	  componentDidMount: function() {
	    NetInfo.isConnected.addEventListener(
	      'change',
	      this._handleConnectivityChange
	    );
	    NetInfo.isConnected.fetch().done(
	      (isConnected) => { this.setState({isConnected}); }
	    );
	  },
	  componentWillUnmount: function() {
	    NetInfo.isConnected.removeEventListener(
	      'change',
	      this._handleConnectivityChange
	    );
	  },
	  _handleConnectivityChange: function(isConnected) {
	    this.setState({
	      isConnected,
	    });
	  },
	  render() {
	    return (
	      <View>
	        <Text>{this.state.isConnected ? 'Online' : 'Offline'}</Text>
	      </View>
	    );
	  }
	});
	exports.title = 'NetInfo';
	exports.description = 'Monitor network status';
	exports.examples = [
	  {
	    title: 'NetInfo.isConnected',
	    description: 'Asyncronously load and observe connectivity',
	    render(): ReactElement { return <IsConnected />; }
	  },
	  {
	    title: 'NetInfo.reachabilityIOS',
	    description: 'Asyncronously load and observe iOS reachability',
	    render(): ReactElement { return <ReachabilityCurrent />; }
	  },
	  {
	    title: 'NetInfo.reachabilityIOS',
	    description: 'Observed updates to iOS reachability',
	    render(): ReactElement { return <ReachabilitySubscription />; }
	  },
    ];
```