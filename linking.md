# iOS 链接 

`LinkingIOS` 给你提供了一个通用接口，用来连接接收和发送应用程序的链接。 

## 基本用法 

### 处理深度链接 

如果你的应用程序是从一个外部链接启动的，并且这个外部链接是注册到你的应用程序里的，那么你就可以利用任意你想要的组件去访问并且处理它

```
    componentDidMount() {
	 var url = LinkingIOS.popInitialURL();
    }
```

在你的应用程序运行期间，如果你也想监听传入应用程序的链接，那么你需要将以下几行添加到你的 `*AppDelegate.m`：

```
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
	  return [RCTLinkingManager application:application openURL:url sourceApplication:sourceApplication annotation:annotation];
    }
```

那么，在你的 React 组件中，你可以监听 `LinkingIOS` 上的事件，如下所示：

```
    componentDidMount() {
	  LinkingIOS.addEventListener('url', this._handleOpenURL);
	},
	componentWillUnmount() {
	  LinkingIOS.removeEventListener('url', this._handleOpenURL);
	},
	_handleOpenURL(event) {
	  console.log(event.url);
    }
```

### 触发应用程序链接 

为了触发一个应用程序的链接（浏览器，电子邮件，或者自定义模式），你需要调用
```
LinkingIOS.openURL(url)
```

如果你想要检查一个已经安装的应用程序是否可以提前处理一个给定的链接，你可以调用

```
    LinkingIOS.canOpenURL(url, (supported) => {
	  if (!supported) {
	    AlertIOS.alert('Can\'t handle url: ' + url);
	  } else {
	    LinkingIOS.openURL(url);
	  }
    });
```

## 方法 

static **addEventListener**(type: string, handler: Function)

通过监听 `url` 事件类型和提供处理程序，将一个处理程序添加到 LinkingIOS changes

static **removeEventListener**(type: string, handler: Function)

通过传递 `url` 事件类型和处理程序，删除一个处理程序

static **openURL**(url: string) 

尝试通过任意已经安装的应用程序打开给定的 `url`

static **canOpenURL**(url: string, callback: Function) 

决定一个已经安装的应用程序是否可以处理一个给定的 `url`，该方法中回调函数将被调用，并且仅通过一个 `bool supported` 的参数。

static **popInitialURL**()

如果应用程序启动是通过一个应用程序链接触发的，那么它将弹出这个链接的 url，否则它将返回 `null`。
