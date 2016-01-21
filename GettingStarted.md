# 入门

## 要求

1. OS X - 当前仅支持 OS X 
2. 推荐使用 [Homebrew](http://brew.sh/) 的方式来安装 nvm，watchman 和 flow。
3. 安装 [Node.js](https://nodejs.org/) 4.0 或者更新的版本。
  - 使用 Homebrew 来安装 **nvm** 或者参考 [它的安装指南](https://github.com/creationix/nvm#installation)。接着运行 `nvm install node && nvm alias default node`, 它可以让您安装最新版本的 Node.js 并设置您的终端，所以你可以通过键入 `node` 来运行它。使用 Nvm 可以让您安装多个版本的 Node.js 并且在它们之间轻松切换。
  - [npm](https://docs.npmjs.com/) 的更新之处。
4. `brew 安装 watchman`。我们推荐您安装 [watchman](https://facebook.github.io/watchman/docs/install.html), 否则您可能在点击一个节点文件的时候出现错误。
5. `brew 安装 flow`。如果您想使用 [flow](http://www.flowtype.org).

我们建议定期运行 `brew update && brew upgrade` 来使您的应用程序保持最新状态。

## 安装 iOS 

我们需要 [Xcode](https://developer.apple.com/xcode/downloads/) 6.3 或者更高的版本。 您可以在 App 应用商店里面安装它。

## 安装 Android 

如果想要编写 React Native 安卓应用程序， 您需要安装安卓 SDK （如果您不想在真机上运行您的应用程序，那么您还需要一个安卓模拟器）。请参阅[安卓安装指南](DevelopmentSetupAndroid.md) 说明来配置你的安卓环境 。

## 快速开始

    $ npm install -g react-native-cli
    $ react-native init AwesomeProject
    $ cd AwesomeProject/

**运行 iOS 应用程序：**

- 在 Xcode 中打开 `ios/AwesomeProject.xcodeproj` 并且点击运行。
- 在选定的文本编辑器中打开 `index.ios.js` 并且编辑代码。
- 点击 iOS 模拟器中的 ⌘-R 来重新加载应用程序并且观察发生的变化！

**运行 Android 应用程序：**

* `$ react-native run-android`
* 在选定的文本编辑器中打开 `index.android.js` 并且编辑代码。
* 按下菜单按钮 （默认情况下是 F2，或在 Genymotion 中点击 ⌘ M），然后选择 * Reload JS * 看看发生了什么变化！
* 在一个终端中运行 `adb logcat *:S ReactNative:V ReactNativeJS:V` 来查看您的应用程序日志。

祝贺你！你已经成功运行并修改了你的第一个 React Native 应用程序。

_如果您在开始的时候遇到任何问题，请参阅 [故障诊断页面](http://facebook.github.io/react-native/docs/troubleshooting.html#content)

## 让安卓支持现有的 React Native 项目   

如果您现在已经拥有一个(iOS) React Native 项目并且想让安卓也支持它, 那么您需要在您现有的项目目录中执行以下命令：

1.将您 `package.json` 文件中的 `react-native`目录更新到[最新的版本](https://www.npmjs.com/package/react-native)
2. `$ npm install`
3. `$ react-native android`
