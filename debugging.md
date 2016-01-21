# 调试 React Native 应用

访问应用程序内开发者菜单：

1. 在 iOS 中摇动设备或在虚拟机里按组合键 `control + ⌘ + z` .
2. 在 Android 中摇动设备或按硬件菜单按钮 （旧的设备中以及大多数虚拟机中都有效，例如， 在 [genymotion](https://www.genymotion.com) 中，你可以按组合键  `⌘ + m` 来模拟点击硬件菜单按钮）

> 提示

> 要禁用产品构建的开发人员菜单:
>
> 1. 在 iOS 中，打开 Xcode 中的项目，选择  `Product` → `Scheme` → `Edit Scheme...` (或按组合键 `⌘ + <`).下一步, 在左边的菜单中选择 `Run` 然后将 Build Configuration 改为 `Release`。
> 2. 在 Android 中， 默认情况下, 由 Gradle 建立发布的开发者菜单将被禁用(例如， Gralde 的 `assembleRelease` 任务)。 虽然这种行为可以通过传递给 `ReactInstanceManager#setUseDeveloperSupport` 正确的值来自定义。

## 重加载   

选择 `Reload` (或者在 iOS 虚拟机中按组合键 `⌘ + r`) 将会重新加载作用于你的应用程序中的 JavaScript 。 如果你增加了新的资源 (例如，将一幅图添加到 iOS  中的 `Images.xcassets` ，或 Android 中的 `res/drawable` 文件夹) 或者对任何本地代码进行修改 ( iOS 中的 Objective-C/Swift 代码或  Android 中的 Java/C++ 代码),你将需要重新生成该应用程序以使更改生效。   

## Chrome 开发工具    

在 Chrome 中调试 JavaScript 代码，在开发者菜单选择 `Debug in Chrome` 。 将打开一个新的标签 [http://localhost:8081/debugger-ui](http://localhost:8081/debugger-ui)。

在 Chrome 中，按下组合键 `⌘ + option + i` 或选择 `View` → `Developer` → `Developer Tools` 切换开发工具控制台。 启用 [捕获异常时暂停](http://stackoverflow.com/questions/2233339/javascript-is-there-a-way-to-get-chrome-to-break-on-all-errors/17324511#17324511) 以获得更佳的调试体验。

在实际设备上进行调试：

1. 在 iOS 中，- 打开文件 `RCTWebSocketExecutor.m` 并更改 `localhost` 为你的电脑IP地址。摇动设备打开开发菜单，选择启动调试。
2. 在 Android 中， 如果你正在运行通过 USB 连接的 Android 5.0+ 设备，您可以使用 `adb` 命令行工具来从设备到您的计算机设置端口转发。 运行： `adb reverse 8081 8081` (参阅 [此链接](http://developer.android.com/tools/help/adb.html) 以获得 `adb` 命令详情)。 或者，你可以打开设备上开发菜单并选择开发设置，然后为设备设置更新调试服务器主机到您的计算机的 IP 地址。

## React 开发工具 (可选)   

安装 [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en) 作为谷歌浏览器的扩展。这将允许您通过 `React` 在开发工具中导航组件层次结构 ( 更多详情参阅 [facebook/react-devtools](https://github.com/facebook/react-devtools) )。

## Live Reload   

这个选项可触发 JS 在连接设备/模拟器上自动刷新。启用此选项：

1. 在 iOS 中，通过开发者菜单选择 `Enable Live Reload` ，当 JavaScript 有任何改动时，应用程序会自动重新加载。
2. 在 Android 中，[启动开发菜单](#debugging-react-native-apps)，进入 `Dev Settings` 并选择 `Auto reload on JS change` 选项。

## FPS (每秒帧数) 显示器   

在 `0.5.0-rc` 以及更高的版本，为了帮助调试性能问题，你可以在开发者菜单启用 FPS 图形叠置。
