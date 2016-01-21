# 相机滚动 

保留所有版权。

源代码是在 BSD-style 许可下经过许可的，是在源代码树的根目录下的 LICENSE 文件里找到的。额外授予的专利权可以在同目录下的 PATENTS 文件里找到。

@flow 

## 方法 

static **saveImageWithTag**(tag: string, successCallback, errorCallback)

利用tag标签保存图像到相机相册。

@param {string} tag - 可以是我们所接受的三种标签中的任意一个：1、url 2、assets-library 标签 3、存储一个图像的内存中返回的标签

static **getPhotos**(params: object, callback: function, errorCallback: function) 

利用来自设备中的本地相机相册的图片识别对象来调用 `callback` 函数，通过 `getPhotosReturnChecker` 来定义匹配类型。

@param {object} params -见 `getPhotosParamChecker` 。@param {function} callback -通过 `getPhotosReturnChecker` 定义自变量的类型调用成功。@param {function} errorCallback - 通过错误消息调用失败。 

## 例子

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/CameraRollExample.ios.js)

```
    'use strict';
	var React = require('react-native');
	var {
	  CameraRoll,
	  Image,
	  SliderIOS,
	  StyleSheet,
	  SwitchIOS,
	  Text,
	  View,
	} = React;
	var CameraRollView = require('./CameraRollView.ios');
	var CAMERA_ROLL_VIEW = 'camera_roll_view';
	var CameraRollExample = React.createClass({
	  getInitialState() {
	    return {
	      groupTypes: 'SavedPhotos',
	      sliderValue: 1,
	      bigImages: true,
	    };
	  },
	  render() {
	    return (
	      <View>
	        <SwitchIOS
	          onValueChange={this._onSwitchChange}
	          value={this.state.bigImages} />
	        <Text>{(this.state.bigImages ? 'Big' : 'Small') + ' Images'}</Text>
	        <SliderIOS
	          value={this.state.sliderValue}
	          onValueChange={this._onSliderChange}
	        />
	        <Text>{'Group Type: ' + this.state.groupTypes}</Text>
	        <CameraRollView
	          ref={CAMERA_ROLL_VIEW}
	          batchSize={5}
	          groupTypes={this.state.groupTypes}
	          renderImage={this._renderImage}
	        />
	      </View>
	    );
	  },
	  _renderImage(asset) {
	    var imageSize = this.state.bigImages ? 150 : 75;
	    var imageStyle = [styles.image, {width: imageSize, height: imageSize}];
	    var location = asset.node.location.longitude ?
	      JSON.stringify(asset.node.location) : 'Unknown location';
	    return (
	      <View key={asset} style={styles.row}>
	        <Image
	          source={asset.node.image}
	          style={imageStyle}
	        />
	        <View style={styles.info}>
	          <Text style={styles.url}>{asset.node.image.uri}</Text>
	          <Text>{location}</Text>
	          <Text>{asset.node.group_name}</Text>
	          <Text>{new Date(asset.node.timestamp).toString()}</Text>
	        </View>
	      </View>
	    );
	  },
	  _onSliderChange(value) {
	    var options = CameraRoll.GroupTypesOptions;
	    var index = Math.floor(value * options.length * 0.99);
	    var groupTypes = options[index];
	    if (groupTypes !== this.state.groupTypes) {
	      this.setState({groupTypes: groupTypes});
	    }
	  },
	  _onSwitchChange(value) {
	    this.refs[CAMERA_ROLL_VIEW].rendererChanged();
	    this.setState({ bigImages: value });
	  }
	});
	var styles = StyleSheet.create({
	  row: {
	    flexDirection: 'row',
	    flex: 1,
	  },
	  url: {
	    fontSize: 9,
	    marginBottom: 14,
	  },
	  image: {
	    margin: 4,
	  },
	  info: {
	    flex: 1,
	  },
	});
	exports.title = '<CameraRollView>';
	exports.description = 'Example component that uses CameraRoll to list user\'s photos';
	exports.examples = [
	  {
	    title: 'Photos',
	    render(): ReactElement { return <CameraRollExample />; }
	  }
    ];
```
