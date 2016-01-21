# 入门

## 需求

1. OS X – 现在这个仓库只包含 iOS 实现，且 Xcode 只能在 Mac 上运行。 
2. 不知道 Xcode 吗？从 Mac App Store 上 [下载它](https://developer.apple.com/xcode/downloads/)。 
3. 安装 node，watchman，flow 的推荐方法是 [Homebrew](http://brew.sh/)。 
4. `brew install node`。不知道 [node](https://nodejs.org/) 和 [npm](https://docs.npmjs.com/)
5. `brew install --HEAD watchman`。我们建议安装 [watchman](https://facebook.github.io/watchman/docs/install.html)，否则你可能会遇到一个有bug的node文件。 
6. `brew install flow`。如果你想使用 [flow](http://www.flowtype.org/)。

## 快速开始

- `npm install -g react-native-cli` 
- `react-native init AwesomeProject`

在新建的文件夹 `AwesomeProject/` 中 

- 打开 `AwesomeProject.xcodeproj`，点击 `Xcode` 中的运行 
- 打开你选择的文本编辑器中的 `index.ios.js`，并且对一些行进行编辑。 
- 在你的 iOS 模拟器中点击 cmd + R([两次](http://openradar.appspot.com/19613391))，看看有什么变化！ 

恭喜！你刚刚成功运行并修改了你的第一个 React Native 应用。 

如果你在开始时遇到任何问题，请看 [故障诊断页面](http://facebook.github.io/react-native/docs/troubleshooting.html#content)。 

