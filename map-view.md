# Map 视图

## Props 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/MapView/MapView.js)

**legalLabelInsets** {顶部：数字型；左部：数字型；底部：数字型；右部：数字型}

为 map 嵌入合法的标签，最初是在 map 的左下角。更多信息请看 `EdgeInsetsPropType.js`。

**maxDelta** 数字型

能够显示的区域的最大尺寸。

**minDelta** 数字型

能够显示的区域的最小尺寸。

**onRegionChange** 函数型

当用户拖动 map 时，会不断地调用回调函数。

**onRegionChangeComplete** 函数型 

当用户完成移动 map 时，调用一次回调函数。

**pitchEnabled** 布尔型

当这个属性设置为 `true`，且有效的相机与 map 相关联，那么相机的螺旋角用于倾斜 map 的平面。当这个属性设置为 `false` 时，相机的螺旋角被忽略，并且 map 上总是显示为好像用户直接向下看。

**region** {纬度：数字型，经度：数字型，latitudeDelta：数字型，longitudeDelta：数字型} 

该地区会被 map 显示出来。

由中心坐标和坐标跨度定义的区域显示出来。

**rotateEnabled** 布尔型

当这个属性设置为 `true`，且有效的相机与 map 相关联，那么相机的航向角用于围绕 map 中心点旋转 map 平面。当该属性设置为 `false` 时，相机的航向角被忽略，map 总是定向的，这样真正的北方就会位于 map 视图的顶部。

**scrollEnabled** 布尔型 

如果是 `false`，用户无法更改 map 显示的区域。默认值是 `true`。

**showsUserLocation** 布尔型 

如果是 `true`，应用程序会请求用户的位置并关注它。默认值是 `false`。

**注意**：为了获取地理位置，你需要添加把 NSLocationWhenInUseUsageDescription 键添加到 info.plist，否则就会失败!

**style**  [View#style](view.md#style)

用于 `MapView` 的样式设置和布局。更多信息请看 `StyleSheet.js` 和 `ViewStylePropTypes.js`。

**zoomEnabled** 布尔型

如果是 `false`，用户无法缩小/放大 map。默认值是 `true`。

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/MapViewExample.js)

``` 
'use strict';
var React = require('react-native');
var StyleSheet = require('StyleSheet');
var {
  MapView,
  Text,
  TextInput,
  View,
} = React;
var MapRegionInput = React.createClass({
  propTypes: {
    region: React.PropTypes.shape({
      latitude: React.PropTypes.number,
      longitude: React.PropTypes.number,
      latitudeDelta: React.PropTypes.number,
      longitudeDelta: React.PropTypes.number,
    }),
    onChange: React.PropTypes.func.isRequired,
  },
  getInitialState: function() {
    return {
      latitude: 0,
      longitude: 0,
      latitudeDelta: 0,
      longitudeDelta: 0,
    };
  },
  componentWillReceiveProps: function(nextProps) {
    this.setState(nextProps.region);
  },
  render: function() {
    var region = this.state;
    return (
      <View>
        <View style={styles.row}>
          <Text>
            {'Latitude'}
          </Text>
          <TextInput
            value={'' + region.latitude}
            style={styles.textInput}
            onChange={this._onChangeLatitude}
          />
        </View>
        <View style={styles.row}>
          <Text>
            {'Longitude'}
          </Text>
          <TextInput
            value={'' + region.longitude}
            style={styles.textInput}
            onChange={this._onChangeLongitude}
          />
        </View>
        <View style={styles.row}>
          <Text>
            {'Latitude delta'}
          </Text>
          <TextInput
            value={'' + region.latitudeDelta}
            style={styles.textInput}
            onChange={this._onChangeLatitudeDelta}
          />
        </View>
        <View style={styles.row}>
          <Text>
            {'Longitude delta'}
          </Text>
          <TextInput
            value={'' + region.longitudeDelta}
            style={styles.textInput}
            onChange={this._onChangeLongitudeDelta}
          />
        </View>
        <View style={styles.changeButton}>
          <Text onPress={this._change}>
            {'Change'}
          </Text>
        </View>
      </View>
    );
  },
  _onChangeLatitude: function(e) {
    this.setState({latitude: parseFloat(e.nativeEvent.text)});
  },
  _onChangeLongitude: function(e) {
    this.setState({longitude: parseFloat(e.nativeEvent.text)});
  },
  _onChangeLatitudeDelta: function(e) {
    this.setState({latitudeDelta: parseFloat(e.nativeEvent.text)});
  },
  _onChangeLongitudeDelta: function(e) {
    this.setState({longitudeDelta: parseFloat(e.nativeEvent.text)});
  },
  _change: function() {
    this.props.onChange(this.state);
  },
});
var MapViewExample = React.createClass({
  getInitialState() {
    return {
      mapRegion: null,
      mapRegionInput: null,
    };
  },
  render() {
    return (
      <View>
        <MapView
          style={styles.map}
          onRegionChange={this._onRegionChanged}
          region={this.state.mapRegion}
        />
        <MapRegionInput
          onChange={this._onRegionInputChanged}
          region={this.state.mapRegionInput}
        />
      </View>
    );
  },
  _onRegionChanged(region) {
    this.setState({mapRegionInput: region});
  },
  _onRegionInputChanged(region) {
    this.setState({
      mapRegion: region,
      mapRegionInput: region,
    });
  },
});
var styles = StyleSheet.create({
  map: {
    height: 150,
    margin: 10,
    borderWidth: 1,
    borderColor: '#000000',
  },
  row: {
    flexDirection: 'row',
    justifyContent: 'space-between',
  },
  textInput: {
    width: 150,
    height: 20,
    borderWidth: 0.5,
    borderColor: '#aaaaaa',
    fontSize: 13,
    padding: 4,
  },
  changeButton: {
    alignSelf: 'center',
    marginTop: 5,
    padding: 3,
    borderWidth: 0.5,
    borderColor: '#777777',
  },
});
exports.title = '<MapView>';
exports.description = 'Base component to display maps';
exports.examples = [
  {
    title: 'Map',
    render(): ReactElement { return <MapViewExample />; }
  },
  {
    title: 'Map shows user location',
    render() {
      return  <MapView style={styles.map} showsUserLocation={true} />;
    }
  }
];
```
