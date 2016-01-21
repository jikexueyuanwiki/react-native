# 列表视图

列表视图——为变化的数据列表的垂直滚动的高效显示而设计的一个核心组件。最小的 API 是创建一个 `ListView.DataSource`，用一个简单的数组数据的 blob 填充，并用那个数据源实例化一个 `ListView` 组件和一个 `renderRow` 回调，它会从数组数据中带走一个 blob 并返回一个可渲染的组件。

最小的例子：

``` 
getInitialState: function() {
  var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
  return {
    dataSource: ds.cloneWithRows(['row 1', 'row 2']),
  };
},
render: function() {
  return (
    <ListView
      dataSource={this.state.dataSource}
      renderRow={(rowData) => <Text>{rowData}</Text>}
    />
  );
},
``` 

列表视图还支持更高级的功能，包括带有 sticky 页眉的部分，页眉和页脚的支持，回调到可用数据的最后(`onEndReached`)和设备窗口变化中可见的行集(onChangeVisibleRows)，以及一些性能优化。

当动态加载一些可能非常大(或概念上无限大的)数据集时，为了让列表视图滚送的顺畅，有一些性能操作设计： 

- 只有重新呈现改变行——提供给数据源的 hasRowChanged 函数告诉列表视图是否需要重新呈现一行，因为源数据发生了变化——更多细节请看 ListViewDataSource。
- 行限速呈现——默认情况下，每次事件循环时，只显示一行 (可用`pageSize`道具定制)。这将工作分解为小块，在呈现行时，减少框架下降的机会。

## Props  

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Libraries/CustomComponents/ListView/ListView.js)

[**ScrollView props...**](scroll-view.md)

**dataSource** 列表视图数据源

**initialListSize** 数字型

多少行呈现在初始组件装置中。使用这个来实现，这样第一个屏幕需要的数据就会一次出现，而不是在多个框架的过程中出现。

**onChangeVisibleRows** 函数型

(visibleRows, changedRows) => void 

当可见的行集改变时调用。`visibleRows` 为所有可见的行映射 `{ sectionID: { rowID: true }}` ，`changedRows` 为已经改变了它们可见性的行映射 `{ sectionID: { rowID: true | false }}`，true 表明行可见，而 false 表明行已经从视图中被删除了。

**onEndReached** 函数型

当所有行已经呈现并且列表被滚动到了 onEndReachedThreshold 的底部时被调用。提供了 native 滚动事件。

**onEndReachedThreshold** 数字型

onEndReached 像素的阈值。

**pageSize** 数字型

每次事件循环显示的行的数量。

**removeClippedSubviews** 布尔型

为提高大型列表滚动性能的实验性能优化，与溢出一起使用：“隐藏”在行容器中。使用时自己承担风险。

**renderFooter** 函数型

() => renderable 

页眉和页脚在每个呈现过程中都会出现(如果提供了这些道具)。如果重新呈现它们耗费很大，那就把它们包在 StaticContainer 或其他适当的机制中。在每一个呈现过程中，页脚始终是在列表的底部，页眉始终在列表的顶部。

**renderHeader** 函数型

**renderRow** 函数型

(rowData, sectionID, rowID) => renderable 需要从数据源和它的 id 取走一个数据条目，并返回一个可渲染的作为行呈现的组件。默认的情况下，数据正是被放入了数据源的东西，但也可以提供自定义的提取器。

**renderSectionHeader** 函数型

(sectionData, sectionID) => renderable

如果提供了该函数，那么本节的 sticky 页眉就会呈现。Sticky 行为意味着它将带着本节顶部的内容滚动，直到它到达屏幕的顶端，此时它会停在屏幕顶部，直到被下一节的页眉推掉。

**scrollRenderAheadDistance** 数字型

在它们以像素的形式出现在屏幕上之前，要多早就开始呈现行。

## 例子 

[Edit on GitHub](https://github.com/facebook/react-native/blob/master/Examples/UIExplorer/ListViewExample.js)

``` 
'use strict';
var React = require('react-native');
var {
  Image,
  ListView,
  TouchableHighlight,
  StyleSheet,
  Text,
  View,
} = React;
var UIExplorerPage = require('./UIExplorerPage');
var ListViewSimpleExample = React.createClass({
  statics: {
    title: '<ListView> - Simple',
    description: 'Performant, scrollable list of data.'
  },
  getInitialState: function() {
    var ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
    return {
      dataSource: ds.cloneWithRows(this._genRows({})),
    };
  },
  _pressData: ({}: {[key: number]: boolean}),
  componentWillMount: function() {
    this._pressData = {};
  },
  render: function() {
    return (
      <UIExplorerPage
        title={this.props.navigator ? null : '<ListView> - Simple'}
        noSpacer={true}
        noScroll={true}>
        <ListView
          dataSource={this.state.dataSource}
          renderRow={this._renderRow}
        />
      </UIExplorerPage>
    );
  },
  _renderRow: function(rowData: string, sectionID: number, rowID: number) {
    var rowHash = Math.abs(hashCode(rowData));
    var imgSource = {
      uri: THUMB_URLS[rowHash % THUMB_URLS.length],
    };
    return (
      <TouchableHighlight onPress={() => this._pressRow(rowID)}>
        <View>
          <View style={styles.row}>
            <Image style={styles.thumb} source={imgSource} />
            <Text style={styles.text}>
              {rowData + ' - ' + LOREM_IPSUM.substr(0, rowHash % 301 + 10)}
            </Text>
          </View>
          <View style={styles.separator} />
        </View>
      </TouchableHighlight>
    );
  },
  _genRows: function(pressData: {[key: number]: boolean}): Array<string> {
    var dataBlob = [];
    for (var ii = 0; ii < 100; ii++) {
      var pressedText = pressData[ii] ? ' (pressed)' : '';
      dataBlob.push('Row ' + ii + pressedText);
    }
    return dataBlob;
  },
  _pressRow: function(rowID: number) {
    this._pressData[rowID] = !this._pressData[rowID];
    this.setState({dataSource: this.state.dataSource.cloneWithRows(
      this._genRows(this._pressData)
    )});
  },
});
var THUMB_URLS = ['https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-ash3/t39.1997/p128x128/851549_767334479959628_274486868_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851561_767334496626293_1958532586_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-ash3/t39.1997/p128x128/851579_767334503292959_179092627_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851589_767334513292958_1747022277_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851563_767334559959620_1193692107_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851593_767334566626286_1953955109_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851591_767334523292957_797560749_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851567_767334529959623_843148472_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851548_767334489959627_794462220_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851575_767334539959622_441598241_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-ash3/t39.1997/p128x128/851573_767334549959621_534583464_n.png', 'https://fbcdn-dragon-a.akamaihd.net/hphotos-ak-prn1/t39.1997/p128x128/851583_767334573292952_1519550680_n.png'];
var LOREM_IPSUM = 'Lorem ipsum dolor sit amet, ius ad pertinax oportere accommodare, an vix civibus corrumpit referrentur. Te nam case ludus inciderint, te mea facilisi adipiscing. Sea id integre luptatum. In tota sale consequuntur nec. Erat ocurreret mei ei. Eu paulo sapientem vulputate est, vel an accusam intellegam interesset. Nam eu stet pericula reprimique, ea vim illud modus, putant invidunt reprehendunt ne qui.';
/* eslint no-bitwise: 0 */
var hashCode = function(str) {
  var hash = 15;
  for (var ii = str.length - 1; ii >= 0; ii--) {
    hash = ((hash << 5) - hash) + str.charCodeAt(ii);
  }
  return hash;
};
var styles = StyleSheet.create({
  row: {
    flexDirection: 'row',
    justifyContent: 'center',
    padding: 10,
    backgroundColor: '#F6F6F6',
  },
  separator: {
    height: 1,
    backgroundColor: '#CCCCCC',
  },
  thumb: {
    width: 64,
    height: 64,
  },
  text: {
    flex: 1,
  },
});
module.exports = ListViewSimpleExample;
```
