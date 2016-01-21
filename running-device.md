# 在设备上运行

注意,在设备上运行需要 [Apple Developer 账号](https://developer.apple.com/register/index.action)，且需要配置你的 iPhone。本指南仅覆盖 React Native 特定的主题。

## 从设备访问开发服务器

你可以使用开发服务器在设备中快速迭代。要做到这一点，你的笔记本电脑和你的手机必须处于相同的 wifi 网络中。

1. 打开 `iOS / AppDelegate.m`
2. 更改 URL 中的 IP，从 `Localhost` 改成你的笔记本电脑的 IP
3. 在 Xcode 中，选择你的手机作为构建目标，并按“构建和运行”

>**提示**  
晃动设备来打开开发菜单(重载、调试等)

## 使用离线包

你也可以将应用程序本身的所有 JavaScript 代码打包。这样你可以在开发服务器没有运行时测试它，并把应用程序提交到到 AppStore。 

1. 打开 `iOS / AppDelegate.m`
2. 遵循“选项 2”的说明： 

	- 取消 `jsCodeLocation =[[NSBundle mainBundle]…` 

	- 在你应用程序的根目录的终端运行给定 `curl` 命令

Packager 支持几个选项： 

- `dev`(默认的 true)——设置了 `__DEV__` 变量的值。当是 `true` 时，它会打开一堆有用的警告。对于产品，它建议使用 `dev = false`。
- `minify`(默认的 false)——只要不通过 UglifyJS 传输 JS 代码。

## 故障排除

如果 `curl` 命令失败，确保 packager 在运行。也尝试在它的结尾添加 `——ipv4` 标志。

如果你刚刚开始了你的项目，`main.jsbundle` 可能不会被包含到 Xcode 项目中。要想添加它,右键单击你的项目目录，然后单击“添加文件……”——选择生成的 `main.jsbundle` 文件。
