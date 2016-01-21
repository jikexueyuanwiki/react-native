# ToolbarAndroid

React 组件，包装了 Android `Toolbar` [小工具](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html)。工具栏可以显示一个标志，导航图标（如汉堡包菜单），标题和副标题和操作列表。标题和子标题被扩展这样以来标志和导航图标显示在左边，标题和副标题在中间并且操作在右边。

如果工具栏具有唯一子级，它将显示在标题和操作之间。

例子：
```java
    render: function() {   
      return (
        <ToolbarAndroid
          logo={require('image!app_logo')}
          title="AwesomeApp"
          actions={[{title: 'Settings', icon: require('image!icon_settings')
    show: 'always'}]}
          onActionSelected={this.onActionSelected} />
     )
    },
    onActionSelected: function(position) {
      if (position === 0) { // index of 'Settings'
     }
    }
```
## 属性 

**actions** [{title: string, icon: Image.propTypes.source, show: enum('always', 'ifRoom', 'never'), showWithText: bool}] 

 将工具栏上的可能动作设置为动作菜单的一部分。这些都显示为图标或小部件右侧的文本。如果不适合，它们将被放置在一个'溢出'菜单。

此属性需要一个对象数组，其中每个对象具有以下键：

- `title`:**必要的**, 这个操作的标题

- `icon`: 这个操作的图标，例如： `require('image!some_icon')`

- `show`:当把这个操作显示为一个图标或隐藏在溢出菜单中时： `always`, `ifRoom` 或 `never`

- `showWithText`: 布尔值，是否显示图标旁边的文本

**logo** Image.propTypes.source 

    设置工具栏的标志。

**navIcon** Image.propTypes.source 

    设置导航图标。

 **onActionSelected** function 

    被选中时调用回调函数。传递到回调的唯一参数是操作数组中的位置。

 **onIconClicked** function 

    在选定图标时调用。

 **subtitle** string 

    设置工具栏副标题。

 **subtitleColor** string 

    设置工具栏副标题的颜色。

 **testID** string 

    用于在端到端测试中查找此视图。

 **title** string 

    设置工具栏标题。

 **titleColor** string 

    设置工具栏副标题的颜色。


### 例子

```java
    'use strict';

    var React = require('react-native');
    var {
      StyleSheet,
      Text,
      View,
    } = React;
    var UIExplorerBlock = require('./UIExplorerBlock');
    var UIExplorerPage = require('./UIExplorerPage');

    var SwitchAndroid = require('SwitchAndroid');
    var ToolbarAndroid = require('ToolbarAndroid');

    var ToolbarAndroidExample = React.createClass({
      statics: {
      title: '<ToolbarAndroid>',
      description: 'Examples of using the Android toolbar.'
    },
    getInitialState: function() {
    return {
      actionText: 'Example app with toolbar component',
      toolbarSwitch: false,
      colorProps: {
        titleColor: '#3b5998',
        subtitleColor: '#6a7180',
       },
     };
    },
    render: function() {
    return (
      <UIExplorerPage title="<ToolbarAndroid>">
        <UIExplorerBlock title="Toolbar with title/subtitle and actions">
          <ToolbarAndroid
            actions={toolbarActions}
            navIcon={require('image!ic_menu_black_24dp')}
            onActionSelected={this._onActionSelected}
            onIconClicked={() => this.setState({actionText: 'Icon clicked'})}
            style={styles.toolbar}
            subtitle={this.state.actionText}
            title="Toolbar" />
          <Text>{this.state.actionText}</Text>
        </UIExplorerBlock>
        <UIExplorerBlock title="Toolbar with logo & custom title view (a View with Switch+Text)">
          <ToolbarAndroid
            logo={require('image!launcher_icon')}
            style={styles.toolbar}>
            <View style={{height: 56, flexDirection: 'row', alignItems: 'center'}}>
              <SwitchAndroid
                value={this.state.toolbarSwitch}
                onValueChange={(value) => this.setState({'toolbarSwitch': value})} />
              <Text>{'\'Tis but a switch'}</Text>
            </View>
          </ToolbarAndroid>
        </UIExplorerBlock>
        <UIExplorerBlock title="Toolbar with no icon">
          <ToolbarAndroid
            actions={toolbarActions}
            style={styles.toolbar}
            subtitle="There be no icon here" />
        </UIExplorerBlock>
        <UIExplorerBlock title="Toolbar with navIcon & logo, no title">
          <ToolbarAndroid
            actions={toolbarActions}
            logo={require('image!launcher_icon')}
            navIcon={require('image!ic_menu_black_24dp')}
            style={styles.toolbar} />
        </UIExplorerBlock>
        <UIExplorerBlock title="Toolbar with custom title colors">
          <ToolbarAndroid
            navIcon={require('image!ic_menu_black_24dp')}
            onIconClicked={() => this.setState({colorProps: {}})}
            title="Wow, such toolbar"
            style={styles.toolbar}
            subtitle="Much native"
            {...this.state.colorProps} />
          <Text>
            Touch the icon to reset the custom colors to the default (theme-provided) ones.
          </Text>
        </UIExplorerBlock>
      </UIExplorerPage>
    );
    },
    _onActionSelected: function(position) {
    this.setState({
      actionText: 'Selected ' + toolbarActions[position].title,
    });
    },
    });

    var toolbarActions = [
    {title: 'Create', icon: require('image!ic_create_black_48dp'), show: 'always'},
    {title: 'Filter'},
    {title: 'Settings', icon: require('image!ic_settings_black_48dp'), show: 'always'},
    ];

    var styles = StyleSheet.create({
    toolbar: {
    backgroundColor: '#e9eaed',
    height: 56,
    },
    });

    module.exports = ToolbarAndroidExample;
```
