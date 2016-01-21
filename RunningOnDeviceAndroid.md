# 在设备上运行(Android)

## USB 调试

在设备上开发最简单的方式就是使用 USB 调试。首先请确保你有 [USB debugging enabled on your device](https://www.google.com/search?q=android+Enable+USB+debugging)。一旦在设备上调试是被允许的，在连接的设备上你可以以同样的方式在模拟器里面使用  `react-native run-android` 来安装并且运行你的 React Native 应用。

## 从设备上获取开发服务器

你也可以在设备上使用开发服务器快速集成。照着下面的描述的步骤之一来给你的设备构建在你的电脑上运行的开发者服务器。

> 注意
>
> 现在绝大多数的安卓设备没有一个我们来触发开发者模式的硬件按钮键。如果是那样的话，你可以通过摇动来开启开发者模式（重新加载，调试等）。

### 使用 adb 反转

> 请注意这个选项只支持运行在安卓 5.0+ (API 21) 上面的设备。

使用 USB 将你的设备连接，并开启调试模式（可以看看上面如何在你的设备上面允许 USB 调试模式）。

1. 运行 `adb reverse tcp:8081 tcp:8081`
2. 你可以使用 `Reload JS` 和其他开发者参数，而不需要额外的配置

### 通过 Wi-Fi 来配置设备并且连接上你的开发者服务器

要做到这一点，你的电脑和你的手机必须在同一个 wifi 网络下。

1. 打开震动菜单 (摇动设备)
2. 前往 `Dev Settings`
3. 前往 `Debug server host for device`
4. 输入该设备的 IP 和 `Reload JS`

