# 图片

一个 react 的组件用以显示不同类型的图片，包括网络图片，静态资源，临时的本地图片，还有本地磁盘的图片，比如手机照片。

举例：

```java
renderImages: function() {
  return (
    <View>
      <Image
        style={styles.icon}
        source={require('image!myIcon')}
      />
      <Image
        style={styles.logo}
        source={{uri: 'http://facebook.github.io/react/img/logo_og.png'}}
      />
    </View>
  );
},
```
### 支撑工具

**onLayout** 函数  
在进行装载和布局改变的时候使用`{nativeEvent: {layout: {x, y, width, height}}}`调用。

**resizeMode** 枚举 ('cover', 'contain', 'stretch')  
当帧与原始图像尺寸不匹配时用于确定如何调整图像的大小。

**source** {uri: string}，编号
`uri` 是一个代表图片资源标识符的字符串，它可以是 http 地址、 本地文件路径或静态图像资源的名称 (它被包含在 `require('image!name')` 函数中) 。

**style** 样式  
       [**Flexbox**......](http://facebook.github.io/react-native/docs/flexbox.html#proptypes)  
       [**Transforms**...](http://facebook.github.io/react-native/docs/transforms.html#proptypes)  
       **resizeMode** Object.keys(ImageResizeMode)   
       **backgroundColor** 字符串  
       **borderColor** 字符串  
       **borderWidth** 数字  
       **borderRadius** 数字  
       **overflow** 枚举('visible', 'hidden')  
       **tintColor** 字符串  
       **opacity** 数字  

**testID** 字符串
一个在 UI 自动测试脚本中使用此元素的唯一标识符。

`ios` **accessibilityLabel** 字符串 

在用户与图像交互时，该文本会由屏幕阅读器读取。 

`ios` **accessible** 布尔值  
当为 true 的时候，指示图像是可访问的元素。

`ios` **capInsets**  {top: number, left: number, bottom: number, right: number}  
当图像的大小被重新调整时，由 capInsets 指定的角落的大小将保持在一个固定的值，但中心内容和图像的边界将被拉伸。这用于创建可调整大小的圆形按钮、 阴影和其他可调整大小的资源。更多关于[苹果的文档](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIImage_Class/index.html#//apple_ref/occ/instm/UIImage/resizableImageWithCapInsets)请点击此处。

`ios` **defaultSource** {uri: string}  
在下载最终图像并且网络断开的时候用来显示的静态图像。

`ios` **onError** 函数  
在加载错误的时候使用 `{nativeEvent: {error}}` 进行调用。

`ios` **onLoadEndr** 函数  
当完全加载成功时进行调用。

`ios` **onLoadEnd** 函数  
不管加载成功还是失败都会调用。

`ios` **onLoadStart** 函数  
加载成功的时候调用。

`ios` **onProgress** 函数  
在下载进程中使用 `{nativeEvent: {loaded, total}}` 进行调用。

### 描述

## 静态资源

在项目的进程中，添加并且移除和处理那些在应用程序不是经常使用的图片是很常见的情况。为了处理这种情况，我们需要找到一个方法来静态地定位那些被用在应用程序里的图片。因此，我们使用了一个标记器。唯一允许的指向 bundle 里的图片的方法就是在源文件中遍历地搜索 `require('image!name-of-the-asset')` 。

```javascript
// GOOD
<Image source={require('image!my-icon')} />

// BAD
var icon = this.props.active ? 'my-icon-active' : 'my-icon-inactive';
<Image source={require('image!' + icon)} />

// GOOD
var icon = this.props.active ? require('image!my-icon-active') : require('image!my-icon-inactive');
<Image source={icon} />
```

当主要的代码执行到这里，你就可以做一些有趣的事情，比如自动将那些用于应用程序的 assets 打包。注意，这些代码不是强制实施的，但不代表将来也不会。

### 使用 Images.xcassets 将静态资源添加到你的 iOS 应用程序中

![](/react-native/img/StaticImageAssets.png)

> **NOTE**: 生成应用程序所需的新资源
>
> 无论在什么时候，您想把新的资源添加到 `Images.xcassets` 中，您都需要在使用它之前通过 Xcode 来重新构建您的应用程序 — — 仅在模拟器内重新加载它是不够的。

*这一进程正在被改进，不久就会提供更好的工作流程。*

### 将静态资源添加到您的 Android 应用程序中

将您的图像作为[位图画板](http://developer.android.com/guide/topics/resources/drawable-resource.html#Bitmap)添加到 android 项目中（`<yourapp>/android/app/src/main/res`）。 为了给您的 assets 文件提供不同的分辨率，使用 [配置限定符](http://developer.android.com/guide/practices/screens_support.html#qualifiers)进行检查。 通常情况下，您将想要把您的 assets 文件放在下列目录 (如果它们不存在，那么在 `res` 下创建它们)：

* `drawable-mdpi` (1x)
* `drawable-hdpi` (1.5x)
* `drawable-xhdpi` (2x)
* `drawable-xxhdpi` (3x)

如果您的 asset 文件丢失了一种分辨率，那么 Android 将采取下一个最好的分辨率并且为您调整它的大小。

> **NOTE**: 生成应用程序所需的新资源
>
> 无论在什么时候您把新的资源添加到您的画板中您都需要在使用它之前通过运行 `react-native run-android` 重新构建您的应用程序 - 仅重新加载 JS 是不够的。

*这一进程正在被改进，不久就会提供更好的工作流程。*

## 网络资源

在您进行编译的时候，许多您的应用程序中需要展示的图片都不能使用，或者你会想要通过加载一些动态图片来保持二进制大小在较低的状态。不像静态资源那样，*您将需要手动指定图像的尺寸*。 

```javascript
// GOOD
<Image source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}}
       style={{width: 400, height: 400}} />

// BAD
<Image source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}} />
```

## 本地文件系统资源

请在 [CameraRoll](/react-native/docs/cameraroll.html) 中查看使用 `Images.xcassets` 之外的本地资源示例 。

### 最好的相片册图片

iOS 的相片册可以让你将同一张图片保存为不同的尺寸，对于选择那张接近你想要尺寸的图片来说，这很重要。你不会想用一张 3264x2448 分辨率的图片作为资源来显示一个 200x200 的缩略图。如果确实有符合你要求的尺寸， React Native 会自动选择它，否则它会使用第一张比特定尺寸大 50% 的图片来避免重新定义尺寸时带来的模糊失真。这些工作 React Native 自动帮你完成了，所以你不必再自己编写乏味和容易出错的代码。

## 为什么不自动调整所有事物？

*在浏览器中* 如果你不给一个图像规定大小，那么浏览器会呈现一个 0x0 的元素、 随后下载图像，然后再以正确的尺寸呈现图像。这种行为最大的问题是，您的 UI 会在正在加载的图像四周跳动，这会造成一个非常糟糕的用户体验。

在 *React Native* 中故意不实施该行为。它的目的是让开发人员可以提前知道远程图像的尺寸 (或图形比例)，我们认为这样做的话可以实现更好的用户体验。通过使用 `require('image!x')` 语法从应用程序包中加载的静态图片可以自动的调整大小，因为它们的尺寸在安装时立即可用。

例如，上述使用 'require('image!logo')'  屏幕截图的结果：

```javascript
{"__packager_asset":true,"isStatic":true,"path":"/Users/react/HelloWorld/iOS/Images.xcassets/react.imageset/logo.png","uri":"logo","width":591,"height":573}
```

## Source 是一个对象类型

在 React Native 中，一个有趣的决定是 `src` 特性将会被命名为 `source`，并且不作为一个字符串而是一个 `uri` 特性的对象类型。

```javascript
<Image source={{uri: 'something.jpg'}} />
```

站在底层来看，这样做的原因是它允许将元数据依附到这个对象中。举个例子，你正在使用 `require('image!icon')`，我们将添加 `isStatic` 作为一个 flag 来标识本地文件（不要依赖这例子，将来这可能会改变！）。这在将来同时也会成为可能，比如我们可能会支持子画面，并用它来取代输出 `{uri: ...}`，我们可以输出 `{uri: ..., crop: {left: 10, top: 50, width: 20, height: 40}}` 同时支持在所有已经存在的网站中透明地显示子画面。

在用户角度上，这会让你用有用的特性比如图片的几何尺寸来注释对象类型，从而计算出将要显示出来的尺寸。尽情地使用这种数据类型来储存你的图片吧。

## 背景图片叠加

一个对于 web 开发者们很常见的需求是 `background-image`。这种情况下，创建一个简单的 `<Image>` 组件然后将它作为子 layer 添加到你想要添加的 layer 上面。

```javascript
return (
  <Image source={...}>
    <Text>Inside</Text>
  </Image>
);
```

## 非主线程加载

图片的解析会花费很多的时间。这是导致网页的帧数下降的其中一个重要的原因，因为解析工作会被执行在主线程中。在 React Native 中，图片的解析会在不同的线程中执行。在实际操作中，你已经处理好这种情况，当图片还没有下载完成，因此需要将 placeholder 显示出来，这不用你写任何代码。

### 举例

```java
'use strict';

var React = require('react-native');
var {
  Image,
  StyleSheet,
  Text,
  View,
  ActivityIndicatorIOS
} = React;

var ImageCapInsetsExample = require('./ImageCapInsetsExample');

var NetworkImageExample = React.createClass({
  watchID: (null: ?number),

  getInitialState: function() {
    return {
      error: false,
      loading: false,
      progress: 0
    };
  },
  render: function() {
    var loader = this.state.loading ?
      <View style={styles.progress}>
        <Text>{this.state.progress}%</Text>
        <ActivityIndicatorIOS style={{marginLeft:5}}/>
      </View> : null;
    return this.state.error ?
      <Text>{this.state.error}</Text> :
      <Image
        source={this.props.source}
        style={[styles.base, {overflow: 'visible'}]}
        onLoadStart={(e) => this.setState({loading: true})}
        onError={(e) => this.setState({error: e.nativeEvent.error, loading: false})}
        onProgress={(e) => this.setState({progress: Math.round(100 * e.nativeEvent.loaded / e.nativeEvent.total)})}
        onLoad={() => this.setState({loading: false, error: false})}>
        {loader}
      </Image>;
  }
});

exports.displayName = (undefined: ?string);
exports.framework = 'React';
exports.title = '<Image>';
exports.description = 'Base component for displaying different types of images.';

exports.examples = [
  {
    title: 'Plain Network Image',
    description: 'If the `source` prop `uri` property is prefixed with ' +
    '"http", then it will be downloaded from the network.',
    render: function() {
      return (
        <Image
          source={{uri: 'http://facebook.github.io/react/img/logo_og.png'}}
          style={styles.base}
        />
      );
    },
  },
  {
    title: 'Plain Static Image',
    description: 'Static assets should be required by prefixing with `image!` ' +
      'and are located in the app bundle.',
    render: function() {
      return (
        <View style={styles.horizontal}>
          <Image source={require('image!uie_thumb_normal')} style={styles.icon} />
          <Image source={require('image!uie_thumb_selected')} style={styles.icon} />
          <Image source={require('image!uie_comment_normal')} style={styles.icon} />
          <Image source={require('image!uie_comment_highlighted')} style={styles.icon} />
        </View>
      );
    },
  },
  {
    title: 'Error Handler',
    render: function() {
      return (
        <NetworkImageExample source={{uri: 'http://TYPO_ERROR_facebook.github.io/react/img/logo_og.png'}} />
      );
    },
    platform: 'ios',
  },
  {
    title: 'Image Download Progress',
    render: function() {
      return (
        <NetworkImageExample source={{uri: 'http://facebook.github.io/origami/public/images/blog-hero.jpg?r=1'}}/>
      );
    },
    platform: 'ios',
  },
  {
    title: 'Border Color',
    render: function() {
      return (
        <View style={styles.horizontal}>
          <Image
            source={smallImage}
            style={[
              styles.base,
              styles.background,
              {borderWidth: 3, borderColor: '#f099f0'}
            ]}
          />
        </View>
      );
    },
    platform: 'ios',
  },
  {
    title: 'Border Width',
    render: function() {
      return (
        <View style={styles.horizontal}>
          <Image
            source={smallImage}
            style={[
              styles.base,
              styles.background,
              {borderWidth: 5, borderColor: '#f099f0'}
            ]}
          />
        </View>
      );
    },
    platform: 'ios',
  },
  {
    title: 'Border Radius',
    render: function() {
      return (
        <View style={styles.horizontal}>
          <Image
            style={[styles.base, {borderRadius: 5}]}
            source={fullImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {borderRadius: 19}]}
            source={fullImage}
          />
        </View>
      );
    },
  },
  {
    title: 'Background Color',
    render: function() {
      return (
        <View style={styles.horizontal}>
          <Image source={smallImage} style={styles.base} />
          <Image
            style={[
              styles.base,
              styles.leftMargin,
              {backgroundColor: 'rgba(0, 0, 100, 0.25)'}
            ]}
            source={smallImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {backgroundColor: 'red'}]}
            source={smallImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {backgroundColor: 'black'}]}
            source={smallImage}
          />
        </View>
      );
    },
  },
  {
    title: 'Opacity',
    render: function() {
      return (
        <View style={styles.horizontal}>
          <Image
            style={[styles.base, {opacity: 1}]}
            source={fullImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {opacity: 0.8}]}
            source={fullImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {opacity: 0.6}]}
            source={fullImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {opacity: 0.4}]}
            source={fullImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {opacity: 0.2}]}
            source={fullImage}
          />
          <Image
            style={[styles.base, styles.leftMargin, {opacity: 0}]}
            source={fullImage}
          />
        </View>
      );
    },
  },
  {
    title: 'Nesting',
    render: function() {
      return (
        <Image
          style={{width: 60, height: 60, backgroundColor: 'transparent'}}
          source={fullImage}>
          <Text style={styles.nestedText}>
            React
          </Text>
        </Image>
      );
    },
  },
  {
    title: 'Tint Color',
    description: 'The `tintColor` style prop changes all the non-alpha ' +
      'pixels to the tint color.',
    render: function() {
      return (
        <View>
          <View style={styles.horizontal}>
            <Image
              source={require('image!uie_thumb_normal')}
              style={[styles.icon, {borderRadius: 5, tintColor: '#5ac8fa' }]}
            />
            <Image
              source={require('image!uie_thumb_normal')}
              style={[styles.icon, styles.leftMargin, {borderRadius: 5, tintColor: '#4cd964' }]}
            />
            <Image
              source={require('image!uie_thumb_normal')}
              style={[styles.icon, styles.leftMargin, {borderRadius: 5, tintColor: '#ff2d55' }]}
            />
            <Image
              source={require('image!uie_thumb_normal')}
              style={[styles.icon, styles.leftMargin, {borderRadius: 5, tintColor: '#8e8e93' }]}
            />
          </View>
          <Text style={styles.sectionText}>
            It also works with downloaded images:
          </Text>
          <View style={styles.horizontal}>
            <Image
              source={smallImage}
              style={[styles.base, {borderRadius: 5, tintColor: '#5ac8fa' }]}
            />
            <Image
              source={smallImage}
              style={[styles.base, styles.leftMargin, {borderRadius: 5, tintColor: '#4cd964' }]}
            />
            <Image
              source={smallImage}
              style={[styles.base, styles.leftMargin, {borderRadius: 5, tintColor: '#ff2d55' }]}
            />
            <Image
              source={smallImage}
              style={[styles.base, styles.leftMargin, {borderRadius: 5, tintColor: '#8e8e93' }]}
            />
          </View>
        </View>
      );
    },
  },
  {
    title: 'Resize Mode',
    description: 'The `resizeMode` style prop controls how the image is ' +
      'rendered within the frame.',
    render: function() {
      return (
        <View style={styles.horizontal}>
          <View>
            <Text style={[styles.resizeModeText]}>
              Contain
            </Text>
            <Image
              style={styles.resizeMode}
              resizeMode={Image.resizeMode.contain}
              source={fullImage}
            />
          </View>
          <View style={styles.leftMargin}>
            <Text style={[styles.resizeModeText]}>
              Cover
            </Text>
            <Image
              style={styles.resizeMode}
              resizeMode={Image.resizeMode.cover}
              source={fullImage}
            />
          </View>
          <View style={styles.leftMargin}>
            <Text style={[styles.resizeModeText]}>
              Stretch
            </Text>
            <Image
              style={styles.resizeMode}
              resizeMode={Image.resizeMode.stretch}
              source={fullImage}
            />
          </View>
        </View>
      );
    },
  },
  {
    title: 'Animated GIF',
    render: function() {
      return (
        <Image
          style={styles.gif}
          source={{uri: 'http://38.media.tumblr.com/9e9bd08c6e2d10561dd1fb4197df4c4e/tumblr_mfqekpMktw1rn90umo1_500.gif'}}
        />
      );
    },
    platform: 'ios',
  },
  {
    title: 'Cap Insets',
    description:
      'When the image is resized, the corners of the size specified ' +
      'by capInsets will stay a fixed size, but the center content and ' +
      'borders of the image will be stretched. This is useful for creating ' +
      'resizable rounded buttons, shadows, and other resizable assets.',
    render: function() {
      return <ImageCapInsetsExample />;
    },
    platform: 'ios',
  },
];

var fullImage = {uri: 'http://facebook.github.io/react/img/logo_og.png'};
var smallImage = {uri: 'http://facebook.github.io/react/img/logo_small_2x.png'};

var styles = StyleSheet.create({
  base: {
    width: 38,
    height: 38,
  },
  progress: {
    flex: 1,
    alignItems: 'center',
    flexDirection: 'row',
    width: 100
  },
  leftMargin: {
    marginLeft: 10,
  },
  background: {
    backgroundColor: '#222222'
  },
  sectionText: {
    marginVertical: 6,
  },
  nestedText: {
    marginLeft: 12,
    marginTop: 20,
    backgroundColor: 'transparent',
    color: 'white'
  },
  resizeMode: {
    width: 90,
    height: 60,
    borderWidth: 0.5,
    borderColor: 'black'
  },
  resizeModeText: {
    fontSize: 11,
    marginBottom: 3,
  },
  icon: {
    width: 15,
    height: 15,
  },
  horizontal: {
    flexDirection: 'row',
  },
  gif: {
    flex: 1,
    height: 200,
  },
});
```
