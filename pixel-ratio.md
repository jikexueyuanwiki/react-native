# 像素比率 

PixelRatio 类为像素密度设备提供了访问权。

这里有一些使用 PixelRatio 的用例： 

### 显示一条和设备许可一样细的线 

宽度 1 实际上相当于 iPhone4+ 的厚度，我们可以使用设定宽度为 `1 / PixelRatio.get()` 的函数来实现。这是一项独立于像素密度的应用在所有设备上的技术。 

```
style={{ borderWidth: 1 / PixelRatio.get() }}
```

### 获取一个正确大小的图像 

如果你使用的是一台像素密度比较高的设备上，那你应该得到一个更高分辨率的图像。一个好的经验法则是在 pixel ratio 上显示多种图像的尺寸。

```
    var image = getImage({
	  width: 200 * PixelRatio.get(),
	  height: 100 * PixelRatio.get()
	});
    <Image source={image} style={{width: 200, height: 100}} />
```

### 方法 

static **get**()

返回设备的像素密度。一些例子：

- PixelRatio.get() === 2
 - iPhone 4, 4S
 - iPhone 5, 5c, 5s
 - iPhone 6
- PixelRatio.get() === 3
 - iPhone 6 plus

### 产品描述 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/docs/PixelRatio.md)

## 像素网格拍摄 

在 iOS 里，你可以为元素指定有任意精度的位置和尺寸，例如29.674825。但是，最终的物理显示就只有一个固定的像素值，例如在 iPhone4 上是 640*960，或者在 iPhone6 上是 750*1334。iOS 试图通过将一个原始的像素扩展成多个值得方法，看似是尽可能忠实于用户的体验价值，实际上是欺骗了众人的眼睛。这项技术的缺点是使得生成的元素看起来很模糊。

实际上，我们发现开发人员并不需要这项功能，但是为了避免生成模糊的像素，他们不得不对它进行手动舍入操作。在 React Native 里，我们都是自动对这些元素进行舍入。

在进行舍入时，我们必须相当的小心。你永远不希望在同一时间使用正常值和四舍五入的值，那就好像你正在不断的积累舍入误差。甚至一个舍入误差会造成致命性的错误，因为一个像素边界可能会消失或者变成两倍那么大。

在 React Native 里，在JS和布局引擎里的一切值都是以一个任意精度的数来进行工作的。这只会发生在当在为主线程里我们进行舍入的原生元素设定任意位置和尺寸的时候。同时，舍入操作是针对根而不是父母完成的，这又一次避免了累积舍入误差。



