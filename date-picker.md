# iOS 日期选择器

使用 `DatePickerIOS` 来在 iOS 上呈现一个日期/时间选择器(selector)。这是一个控制组件，所以为了组件更新，你必须钩在 `onDateChange` 回调中，并更新 `date` 支持，否则用户的变化将立即恢复以反映 `props.date`。

## Props  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/Components/DatePicker/DatePickerIOS.ios.js)

**date** 日期型

当前选中的日期。

**maximumDate** 日期型

最大的日期。

限制可能的日期/时间值的范围。

**minimumDate** 日期型

最小的日期。

限制了可能的日期/时间值的范围。

**minuteInterval** 枚举型(1,2,3,4,5,6,10,12,15,20,30)

可选择的分钟的间隔。

**mode** 枚举型(“date”,“time”,“datetime”)

日期选择器模式。

**onDateChange** 函数型

日期变更处理程序。

当用户更改了 UI 的日期或时间时，它就会被调用。第一个也是唯一一个参数是一个 Date 对象，代表了新的日期和时间。

**timeZoneOffsetInMinutes** 数字型

在几分钟内时区偏移。

默认情况下，日期选择器将使用设备的时区。有了这个参数，才有可能迫使某个时区偏移。例如，为了显示太平洋的标准时间，传递 -7 * 60。

## 例子  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/DatePickerIOSExample.js)

``` 
'use strict';
var React = require('react-native');
var {
  DatePickerIOS,
  StyleSheet,
  Text,
  TextInput,
  View,
} = React;
var DatePickerExample = React.createClass({
  getDefaultProps: function () {
    return {
      date: new Date(),
      timeZoneOffsetInHours: (-1) * (new Date()).getTimezoneOffset() / 60,
    };
  },
  getInitialState: function() {
    return {
      date: this.props.date,
      timeZoneOffsetInHours: this.props.timeZoneOffsetInHours,
    };
  },
  onDateChange: function(date) {
    this.setState({date: date});
  },
  onTimezoneChange: function(event) {
    var offset = parseInt(event.nativeEvent.text, 10);
    if (isNaN(offset)) {
      return;
    }
    this.setState({timeZoneOffsetInHours: offset});
  },
  render: function() {
    // Ideally, the timezone input would be a picker rather than a
    // text input, but we don't have any pickers yet :(
    return (
      <View>
        <WithLabel label="Value:">
          <Text>{
            this.state.date.toLocaleDateString() +
            ' ' +
            this.state.date.toLocaleTimeString()
          }</Text>
        </WithLabel>
        <WithLabel label="Timezone:">
          <TextInput
            onChange={this.onTimezoneChange}
            style={styles.textinput}
            value={this.state.timeZoneOffsetInHours.toString()}
          />
          <Text> hours from UTC</Text>
        </WithLabel>
        <Heading label="Date + time picker" />
        <DatePickerIOS
          date={this.state.date}
          mode="datetime"
          timeZoneOffsetInMinutes={this.state.timeZoneOffsetInHours * 60}
          onDateChange={this.onDateChange}
        />
        <Heading label="Date picker" />
        <DatePickerIOS
          date={this.state.date}
          mode="date"
          timeZoneOffsetInMinutes={this.state.timeZoneOffsetInHours * 60}
          onDateChange={this.onDateChange}
        />
        <Heading label="Time picker, 10-minute interval" />
        <DatePickerIOS
          date={this.state.date}
          mode="time"
          timeZoneOffsetInMinutes={this.state.timeZoneOffsetInHours * 60}
          onDateChange={this.onDateChange}
          minuteInterval={10}
        />
      </View>
    );
  },
});
var WithLabel = React.createClass({
  render: function() {
    return (
      <View style={styles.labelContainer}>
        <View style={styles.labelView}>
          <Text style={styles.label}>
            {this.props.label}
          </Text>
        </View>
        {this.props.children}
      </View>
    );
  }
});
var Heading = React.createClass({
  render: function() {
    return (
      <View style={styles.headingContainer}>
        <Text style={styles.heading}>
          {this.props.label}
        </Text>
      </View>
    );
  }
});
exports.title = '<DatePickerIOS>';
exports.description = 'Select dates and times using the native UIDatePicker.';
exports.examples = [
{
  title: '<DatePickerIOS>',
  render: function(): ReactElement {
    return <DatePickerExample />;
  },
}];
var styles = StyleSheet.create({
  textinput: {
    height: 26,
    width: 50,
    borderWidth: 0.5,
    borderColor: '#0f0f0f',
    padding: 4,
    fontSize: 13,
  },
  labelContainer: {
    flexDirection: 'row',
    alignItems: 'center',
    marginVertical: 2,
  },
  labelView: {
    marginRight: 10,
    paddingVertical: 2,
  },
  label: {
    fontWeight: '500',
  },
  headingContainer: {
    padding: 4,
    backgroundColor: '#f6f7f8',
  },
  heading: {
    fontWeight: '500',
    fontSize: 14,
  },
});
```
