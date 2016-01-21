# iOS 推送通知 

为你的应用程序处理推送通知，包括权限的处理和图标标记数量。

为了启动和运行，[在Apple上配置您的通知](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html) 和服务器端系统。为了得到一个想法，这里是 [解析指南](https://parse.com/tutorials/ios-push-notifications)。 

## 方法 

static **setApplicationIconBadgeNumber**(number: number)

在主屏幕上为应用程序的图标设置标记数量

static **getApplicationIconBadgeNumber**(callback: Function)

在主屏幕上为应用程序的图标获取当前的标记数量

static **addEventListener**(type: string, handler: Function)

当应用程序在前台或者后台运行的时候，为了远程通知链接一个监听器。

处理程序将会以一个 `PushNotificationIOS` 的实例的形式被调用

static **requestPermissions**() 

从iOS上请求所有的通知权限，提示用户对话框

static **checkPermissions**(callback: Function)

查看当前正被启用的推送权限。`Callback` 函数将被一个 
`permission` 的对象调用：

- alert :boolean
- badge :boolean
- sound :boolean

static **removeEventListener**(type: string, handler: Function) 

删除事件监听器。为了防止内存泄露，该操作在 `componentWillUnmount` 里完成。

static **popInitialNotification**()

如果应用程序从一个通知被冷发射，那么一个原始通知将变成可用状态。 
`popInitialNotification` 的第一个调用者将获取最初的通知对象，或者为 `null`。后续的调用将返回 null。

**constructor**(nativeNotif) 

你自己可能永远都不需要 instansiate `PushNotificationIOS`。你只需要监听 `notification` 事件并且调用 `popInitialNotification`就足够了。

**getMessage**()

`getAlert` 的一个别名，该函数是为了获取通知的主要消息字符串

**getSound**()

从 `aps` 对象中获取声音字符串

**getAlert**()

从 `aps` 对象中获取通知的主要消息字符串

**getBadgeCount**()

从 `aps` 对象中获取标记数量

**getData**()

在通知上获取数据对象 

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/PushNotificationIOSExample.js)

```
    'use strict';
	var React = require('react-native');
	var {
	  AlertIOS,
	  PushNotificationIOS,
	  StyleSheet,
	  Text,
	  TouchableHighlight,
	  View,
	} = React;
	var Button = React.createClass({
	  render: function() {
	    return (
	      <TouchableHighlight
	        underlayColor={'white'}
	        style={styles.button}
	        onPress={this.props.onPress}>
	        <Text style={styles.buttonLabel}>
	          {this.props.label}
	        </Text>
	      </TouchableHighlight>
	    );
	  }
	});
	class NotificationExample extends React.Component {
	  componentWillMount() {
	    PushNotificationIOS.addEventListener('notification', this._onNotification);
	  }
	  componentWillUnmount() {
	    PushNotificationIOS.removeEventListener('notification', this._onNotification);
	  }
	  render() {
	    return (
	      <View>
	        <Button
	          onPress={this._sendNotification}
	          label="Send fake notification"
 	       />
	      </View>
	    );
	  }
	  _sendNotification() {
	    require('RCTDeviceEventEmitter').emit('remoteNotificationReceived', {
	      aps: {
	        alert: 'Sample notification',
	        badge: '+1',
	        sound: 'default',
	        category: 'REACT_NATIVE'
	      },
	    });
 	 }
	  _onNotification(notification) {
	    AlertIOS.alert(
	      'Notification Received',
	      'Alert message: ' + notification.getMessage(),
	      [{
	        text: 'Dismiss',
	        onPress: null,
	      }]
	    );
	  }
	}
	class NotificationPermissionExample extends React.Component {
	  constructor(props) {
	    super(props);
	    this.state = {permissions: null};
	  }
	  render() {
	    return (
	      <View>
	        <Button
	          onPress={this._showPermissions.bind(this)}
	          label="Show enabled permissions"
	        />
	        <Text>
	          {JSON.stringify(this.state.permissions)}
	        </Text>
	      </View>
	    );
	  }
	  _showPermissions() {
	    PushNotificationIOS.checkPermissions((permissions) => {
	      this.setState({permissions});
	    });
	  }
	}
	var styles = StyleSheet.create({
	  button: {
	    padding: 10,
	    alignItems: 'center',
	    justifyContent: 'center',
	  },
	  buttonLabel: {
	    color: 'blue',
	  },
	});
	exports.title = 'PushNotificationIOS';
	exports.description = 'Apple PushNotification and badge value';
	exports.examples = [
	{
	  title: 'Badge Number',
	  render(): React.Component {
	    PushNotificationIOS.requestPermissions();
	    return (
 	     <View>
	        <Button
	          onPress={() => PushNotificationIOS.setApplicationIconBadgeNumber(42)}
	          label="Set app's icon badge to 42"
	        />
	        <Button
	          onPress={() => PushNotificationIOS.setApplicationIconBadgeNumber(0)}
	          label="Clear app's icon badge"
	        />
	      </View>
	    );
	  },
	},
	{
	  title: 'Push Notifications',
	  render(): React.Component {
	    return <NotificationExample />;
	  }
	},
	{
	  title: 'Notifications Permissions',
	  render(): React.Component {
	    return <NotificationPermissionExample />;
	  }
    }];
```


