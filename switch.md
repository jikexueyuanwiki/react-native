# iOS 开关 

使用 `SwitchIOS` 在 iOS 上呈现出布尔型的输入。这是一个控件组件，所以为了更新组件，你必须使用 `onValueChange` 回调并且更新值`value`。否则的话用户的改变会被立即反映到 `props.value` ，这是一个真理。 

## 属性  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/SwitchIOS/SwitchIOS.ios.js)

**disabled** 布尔型

如果值为真，那么用户将不能切换开关。默认值为假。

**onTintColor** 字符串型

当开关打开时候的背景颜色。

**onValueChange** 函数

当用户切换开关时，调用回调函数。

**thumbTintColor** 字符串型

开关按钮的背景颜色。

**tintColor** 字符串型

当开关关闭后的背景颜色。

**value** 布尔型

开关的值，如果为真，开关会打开。默认值为假。

## 例子  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/SwitchIOSExample.js)

``` 
'use strict';
var React = require('react-native');
var {
  SwitchIOS,
  Text,
  View
} = React;
var BasicSwitchExample = React.createClass({
  getInitialState() {
    return {
      trueSwitchIsOn: true,
      falseSwitchIsOn: false,
    };
  },
  render() {
    return (
      <View>
        <SwitchIOS
          onValueChange={(value) => this.setState({falseSwitchIsOn: value})}
          style={{marginBottom: 10}}
          value={this.state.falseSwitchIsOn} />
        <SwitchIOS
          onValueChange={(value) => this.setState({trueSwitchIsOn: value})}
          value={this.state.trueSwitchIsOn} />
      </View>
    );
  }
});
var DisabledSwitchExample = React.createClass({
  render() {
    return (
      <View>
        <SwitchIOS
          disabled={true}
          style={{marginBottom: 10}}
          value={true} />
        <SwitchIOS
          disabled={true}
          value={false} />
      </View>
    );
  },
});
var ColorSwitchExample = React.createClass({
  getInitialState() {
    return {
      colorTrueSwitchIsOn: true,
      colorFalseSwitchIsOn: false,
    };
  },
  render() {
    return (
      <View>
        <SwitchIOS
          onValueChange={(value) => this.setState({colorFalseSwitchIsOn: value})}
          onTintColor="#00ff00"
          style={{marginBottom: 10}}
          thumbTintColor="#0000ff"
          tintColor="#ff0000"
          value={this.state.colorFalseSwitchIsOn} />
        <SwitchIOS
          onValueChange={(value) => this.setState({colorTrueSwitchIsOn: value})}
          onTintColor="#00ff00"
          thumbTintColor="#0000ff"
          tintColor="#ff0000"
          value={this.state.colorTrueSwitchIsOn} />
      </View>
    );
  },
});
var EventSwitchExample = React.createClass({
  getInitialState() {
    return {
      eventSwitchIsOn: false,
      eventSwitchRegressionIsOn: true,
    };
  },
  render() {
    return (
      <View style={{ flexDirection: 'row', justifyContent: 'space-around' }}>
        <View>
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchIsOn} />
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchIsOn} />
            <Text>{this.state.eventSwitchIsOn ? "On" : "Off"}</Text>
        </View>
        <View>
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchRegressionIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchRegressionIsOn} />
          <SwitchIOS
            onValueChange={(value) => this.setState({eventSwitchRegressionIsOn: value})}
            style={{marginBottom: 10}}
            value={this.state.eventSwitchRegressionIsOn} />
          <Text>{this.state.eventSwitchRegressionIsOn ? "On" : "Off"}</Text>
        </View>
      </View>
    );
  }
});
exports.title = '<SwitchIOS>';
exports.displayName = 'SwitchExample';
exports.description = 'Native boolean input';
exports.examples = [
  {
    title: 'Switches can be set to true or false',
    render(): ReactElement { return <BasicSwitchExample />; }
  },
  {
    title: 'Switches can be disabled',
    render(): ReactElement { return <DisabledSwitchExample />; }
  },
  {
    title: 'Custom colors can be provided',
    render(): ReactElement { return <ColorSwitchExample />; }
  },
  {
    title: 'Change events can be detected',
    render(): ReactElement { return <EventSwitchExample />; }
  },
  {
    title: 'Switches are controlled components',
    render(): ReactElement { return <SwitchIOS />; }
  }
];
```
