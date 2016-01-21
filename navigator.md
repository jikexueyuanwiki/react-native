# 导航器

在你的应用程序中使用 `Navigator` 来在不同场景之间过渡。为了实现这一功能，为导航器提供了路由对象来识别每一个场景，还提供了一个 `renderScene` 函数，导航器可以用它来为给定的路线渲染场景。

为了改变场景的动画或动作属性，提供了一个 `configureScene` 道具来为给定的路由配置对象。看到导航器。默认动画及更多的关于场景配置选项的信息，请看 `Navigator.SceneConfigs`。

## 基本的使用

``` 
<Navigator
    initialRoute={{name: 'My First Scene', index: 0}}
    renderScene={(route, navigator) =>
      <MySceneComponent
        name={route.name}
        onForward={() => {
          var nextIndex = route.index + 1;
          navigator.push({
            name: 'Scene ' + nextIndex,
            index: nextIndex,
          });
        }}
        onBack={() => {
          if (route.index > 0) {
            navigator.pop();
          }
        }}
      />
    }
  />
```

## 导航方法

`Navigator` 有两种方式进行导航。如果你有一个参考元素，你可以调用一些方法来触发导航：

- `jumpBack()`——在不需要卸载当前场景的情况下向后跳
- `jumpForward()`——向前跳转到路线堆栈中的下一个场景
-` jumpTo(route)`——过渡到一个现有的没有被卸载的场景
- `push(route)`——导航到一个新的场景，挤压任何你能够 `jumpForward` 的场景
-` pop()`——回归并卸载当前场景
-` replace(route)`——用一个新路线替换当前场景
- `replaceAtIndex(route,index)——取代一个由索引指定的场景
- `replacePrevious(route)`——取代之前的场景
- `immediatelyResetRouteStack(routeStack)`——用一组路线重置每个场景
- `popToRoute(route)`——弹出一个由它的路线指定的特定的场景。这之后所有的场景将被卸载
- `popToTop()`——弹出堆栈中的第一个场景，卸载其他场景

## 导航器对象

通过 `renderScene` 函数 navigator 对象对场景是可用的。对象有所有的导航方法，以及一些实用程序:

- `parentNavigator`——父导航对象的参考，在 props.navigator 中被传递
- `onWillFocus`——用来向父导航器传递一个导航焦点事件
- `onDidFocus`——用来向父导航器传递一个导航焦点事件

## Props 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/CustomComponents/Navigator/Navigator.js)

**configureScene** 函数型

可选功能，允许配置场景动画和手势。将由路线调用，且应该返回一个场景配置对象

``` 
(route) => Navigator.SceneConfigs.FloatFromRight
```

**initialRoute** 对象型

提供一个单一的“路线”来开始。路线是一个任意的对象，导航器将使用它在场景呈现之前确定每个场景。initialRoute 或 initialRouteStack 是必需的。

**initialRouteStack** [对象型] 

提供一组路线来呈现最初的场景。如果没有提供 initialRoute，那么该道具就会被需求。

**navigationBar** 节点型 

以可选的方式提供一个能够存留在场景之间转换的导航栏

**navigator** 对象型 

以可选的方式从父导航器提供 navigator 对象

**onDidFocus** 函数型

在场景过渡完成后或在最初安装后该函数会被每个场景的新路线调用。这将覆盖在 this.props.navigator 的onDidFocus处理程序上。

**onItemRef** 函数型

当场景参考发生变化时，该函数会被调用 (ref，indexInStack)

**onWillFocus** 函数型

将在安装中和每个导航转换之前发射目标路线，覆盖this.props.navigator中的处理程序。这将覆盖this.props.navigator 中的 onDidFocus 处理程序

**renderScene** 函数型

对于一个给定的路线哪一个场景会出现需要该函数。将由路线和 navigator 对象调用。

``` 
(route, navigator) =>
  <MySceneComponent title={route.title} /> 
```

**sceneStyle** [View#style](view.md#style) 

设置样式，以应用于每个场景的容器中。
