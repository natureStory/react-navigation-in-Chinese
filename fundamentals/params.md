# 将参数传递给路由

现在我们已经知道如何创建带有一些路由的 StackNavigator 并在这些路由之间导航跳转，
我们来看看如何在导航跳转时将数据传递给路由。

有两点：
* 通过将参数放入一个对象作为 navigation.navigate 函数的第二个参数传递给路由：this.props.navigation.navigate('RouteName', { /* 参数设置在这里 */ })
* 在您的屏幕组件中读取参数：this.props.navigation.state.params。或者，如果您想直接访问参数（例如，通过this.props.itemId），那么可以使用社区开发的 [react-navigation-props-mapper](https://github.com/vonovak/react-navigation-props-mapper) 软件包。

```js
class HomeScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
        <Button
          title="Go to Details"
          onPress={() => {
            /* 1. Navigate to the Details route with params */
            this.props.navigation.navigate('Details', {
              itemId: 86,
              otherParam: 'anything you want here',
            });
          }}
        />
      </View>
    );
  }
}

class DetailsScreen extends React.Component {
  render() {
    /* 2. Read the params from the navigation state */
    const { params } = this.props.navigation.state;
    const itemId = params ? params.itemId : null;
    const otherParam = params ? params.otherParam : null;

    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Details Screen</Text>
        <Text>itemId: {JSON.stringify(itemId)}</Text>
        <Text>otherParam: {JSON.stringify(otherParam)}</Text>
        <Button
          title="Go to Details... again"
          onPress={() => this.props.navigation.navigate('Details')}
        />
        <Button
          title="Go back"
          onPress={() => this.props.navigation.goBack()}
        />
      </View>
    );
  }
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/navigate-with-params)

# 概要

* navigate 接受可选的第二个参数，让您将参数传递给您正在导航的路由。例如：this.props.navigation.navigate('RouteName', {paramName: 'value'}) 将一条附带 {paramName: 'value'} 的新路由推入到 StackNavigator。
* 你可以通过 this.props.navigation.state.params 读取参数。如果没有指定参数则值为null。

[【← 跳转】](./navigating.md)      [【配置标题栏 →】](./headers.md)
