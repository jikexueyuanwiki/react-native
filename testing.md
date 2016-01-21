# 测试
 

## 运行测试和贡献

React Native 回购有几个你可以运行的测试，来验证你没有用PR引起拟合。这些测试是用 [Travis](http://docs.travis-ci.com/) 持续集成系统运行的，并自动的向你的 PR 发布结果。你也可以在 IntegrationTest 和在 Xcode 中的 UIExplorer 应用中，使用 cmd+U 本地运行。您可以通过在命令行的 `npm test` 运行 jest 测试。但是我们目前还没有很大的测试覆盖率，所以大多数的变化仍将需要大量手工验证，但如果你想帮助我们提高我们的测试覆盖率，我们是非常欢迎的!

## Jest 测试

[Jest](http://facebook.github.io/jest/) 测试是 JS-only 测试，运行在节点命令行上。测试位于它们测试的文件 `__tests__` 目录中，还有一个对不是位于故障隔离和最大速度测试下的积极模拟功能的强调。你可以用来自 react-native 根的 `npm test` 运行现有的 React Native jest 测试，并且我们鼓励你为你想做出贡献的任何组件添加你自己的测试。基本示例请看 `getImageSource-test.js`。

## 集成测试

React Native 提供设施，使测试需要 native 和 JS 组件进行跨桥交互的集成组件更容易。两个主要组件是 `RCTTestRunner` 和 `RCTTestModule`。`RCTTestRunner` 设置了 React Native 环境并提供设备运行测试，正如在 Xcode 中的 `XCTestCase`(` runTest:module` 是最简单的方法)。`RCTTestModule` 和 `TestModule` 一样，通过 `NativeModules` 被导出到 JS 中。测试写在 JS 中，当它们完成时，必须调用 `TestModule.markTestCompleted()`，否则测试将超时失败。测试失败主要是通过抛出异常表示。它还可以用 `runTest:module:initialProps:expectErrorRegex:` 或 `runTest:module:initialProps:expectErrorBlock:` 测试错误条件，它预计抛出一个错误并验证错误与提供的标准相匹配。对于例子的使用，请看 `IntegrationTestHarnessTest.js` 和 `IntegrationTestsTests.m`。

## 快照测试

常见的一种集成测试是快照测试。这些测试渲染一个组件，并使用 `TestModule.verifySnapshot()` 验证参考图像的屏幕快照，在幕后使用 `FBSnapshotTestCase` 库。参考图像通过在 `RCTTestRunner` 中设置 `recordMode = YES` 被记录下来，然后运行测试。快照在 32 位和 64 位系统中略有不同，且在不同的操作系统版本中也有所不同，所以建议你使用正确的配置运行测试。同时强烈建议所有网络数据被模拟，以及其他潜在的麻烦的依赖性。基本示例请看 `SimpleSnapshotTest`。
