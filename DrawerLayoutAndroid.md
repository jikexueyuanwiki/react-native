#  DrawerLayoutAndroid

React 组件封装平台 `DrawerLayout`（仅适用于Android）。Drawer（通常用于导航）呈现 `renderNavigationView` 渲染导航视图和直接子级，是呈现（您的内容）的主要视图。导航视图是最初在屏幕上不可见的，但可以从由 `drawerPosition` 指定的窗口的侧面拉出，其宽度可通过 `drawerWidth` 设置。

例如：   

```java
    render: function() {      
      var navigationView = (     
       		<Text style={{margin: 10, fontSize: 15, textAlign: 'left'}}>I'm in the   
    Drawer!</Text  
      );  
      return (  
    <DrawerLayoutAndroid  
      drawerWidth={300}  
      drawerPosition={DrawerLayoutAndroid.positions.Left}  
      renderNavigationView={() =navigationView} 
    	  <Text style={{10, fontSize: 15, textAlign: 'right'}}>Hello</Text>
      <Text style={{10, fontSize: 15, textAlign: 'right'}}>World!</Text>
    </DrawerLayoutAndroid>
      );  
    },
 ```  
 
## 属性  
                        
**drawerPosition** enum(DrawerConsts.DrawerPosition.Left, DrawerConsts.DrawerPosition.Right)

    指定 drawer 将从屏幕的一侧滑动。
    
**drawerWidth** number 

    指定 drawer 的宽度，即从窗口的边缘拉到视图更精确的宽度。
**keyboardDismissMode** enum('none', "on-drag") 

    确定键盘是否响应拖动被驳回。
    -'none' (默认值), 拖动不影响键盘。
    -'on-drag', 拖动开始，键盘被驳回。

**onDrawerClose** 函数 

    导航视图关闭时调用函数。

**onDrawerOpen** 函数  

    导航视图打开时调用函数。

**onDrawerSlide** 函数  

    与导航视图交互时调用函数。

**onDrawerStateChanged** 函数 

    当 Drawer 状态发生变化时调用函数，drawer 有 3 种状态:
    - idle, 表示与导航视图没有交互
    - dragging,表示目前有与导航视图的交互 
    - settling, 表示有与导航视图的交互，并且导航视图正在的关闭或打开。

**renderNavigationView** 函数 

    导航图将被渲染到屏幕的一侧，并且可以拉出。


