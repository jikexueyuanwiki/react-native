# 应用程序注册表 

`AppRegistry` 是运行所有 React Native 应用程序的 JS 入口点。应用程序跟组件需要通过 `AppRegistry.registerComponent` 来注册它们自身，然后本地系统就可以加载应用程序的包，再然后当 `AppRegistry.runApplication`准备就绪后就可以真正的运行该应用程序了。

`AppRegistry` 在 `require` 序列里是 `required`，确保在其他模块被需求之前 JS 执行环境已经被 `required`。 

## 方法 

```
static **registerConfig**(config: Array<AppConfig>) 

static **registerComponent**(appKey: string, getComponentFunc: Function) 

static **registerRunnable**(appKey: string, func: Function) 

static **runApplication**(appKey: string, appParameters: any)
```
