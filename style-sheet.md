# 样式表 

一个样式表是一个类似于CSS样式表的抽象体

创建一个新的样式表：

```
    var styles = StyleSheet.create({
	  container: {
	    borderRadius: 4,
	    borderWidth: 0.5,
	    borderColor: '#d6d7da',
	  },
	  title: {
	    fontSize: 19,
	    fontWeight: 'bold',
	  },
	  activeTitle: {
	    color: 'red',
	  },
    });
```

使用样式表：

```
    <View style={styles.container}>
	  <Text style={[styles.title, this.props.isActive && styles.activeTitle]} />
    </View>
```

代码质量：

-	通过移动样式渲染功能，你可以是你的代码理解起来更容易。
-	对样式进行命名，对在渲染功能的低水平组件中添加意义是一个很好的方式。

性能：

-	在样式对象中使用一个样式表可以使得通过ID对它进行参考成为可能，而不是每一次都创建一个新的样式对象。
-	它还允许通过桥梁对样式进行一次发送。所有后续的使用都是通过id（尚未实施）。 

## 方法 

static **create**(obj: {[key: string]: any})

