# 安装 Android 运行环境

本指南描述了在安卓模拟器上运行 React Native 安卓应用程序所需要的开发环境的基本安装步骤。在这里我们不讨论诸如 IDE 的开发工具配置。

这些指南只包含了从头开始安装的过程。如果你恰好有一些旧的、 过时的 Android SDK 版本，请务必把所需的包更新至下面提到的版本并安装所有缺少的部分。

## 安装和配置 SDK

1. [安装最新的 JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
2. 使用 `brew install android-sdk` 来安装安卓 SDK。
3. 将它添加到 `~/.bashrc`, `~/.zshrc` 或者任何其他您的 shell 所使用的路径：
        export ANDROID_HOME=/usr/local/opt/android-sdk
4. 启动一个新的 shell 并且运行 `android`；在出现窗口中请检查：
  * Android SDK 生成工具版本 23.0.1
  * Android 6.0 (API 23)
  * Android Support Repository
5. 点击 "Install Packages".

## 安装和运行 Android 原生模拟器

1. 启动一个新的 shell 并且运行 `android`;在出现窗口中请检查：
  * Intel x86 原生系统映像 （支持 Android 5.1.1 - API 22）
  * Intel x86 仿真器加速器 （HAXM 安装）
  ![SDK 管理器窗口](images/AndroidSDK1.png)       
  
  ![SDK 管理器窗口](images/AndroidSDK2.png)
2. 点击 "Install Packages".
3. [配置 HAXM](http://developer.android.com/tools/devices/emulator.html#vm-mac).
4. 使用安卓模拟器创建一个 Android 的虚拟设备 (AVD)：
  1. 运行 `android avd` 并且点击 **Create...**
  ![创建 AVD 对话框](images/CreateAVD.png)
  2. 选定该新的 AVD, 并且点击 `Start...`
