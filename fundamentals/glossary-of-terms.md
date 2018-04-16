# 专业术语

这是文档的新部分，
它缺少很多术语！
请[提交请求或问题](https://github.com/react-navigation/react-navigation.github.io)并附上您认为应在此解释的术语。

### Header (标题栏)

也称为导航标题，导航栏，头部，可能还有很多其他的东西。这是屏幕顶部的矩形，其中包含后退按钮和屏幕标题。整个矩形通常称为React Navigation中的标题。

### Screen component (屏幕组件)

屏幕组件是我们在路由配置中使用的组件。

```js
const RootStack = StackNavigator(
  {
    Home: {
      screen: HomeScreen,    // <----
    },
    Details: {
      screen: DetailsScreen, // <----
    },
  },
  {
    initialRouteName: 'Home',
  }
);
```

Screen 组件名称中的后缀完全是可选的，
但是这是一个常用的约定; 
我们可以称之为 CasaPantalla(没有想到一个合适的对应翻译) 而且这种方法也是一样的。

我们之前看到屏幕组件是由 navigator prop 提供的。
需要注意的是，
这只会发生在通过 React Navigation(例如，响应this.props.navigation.navigate)渲染路由组件的时候。
例如，
如果我们把 DetailsScreen 渲染为 HomeScreen 的子组件，
则 DetailsScreen 不会提供 navigation prop，
并且当您在主屏幕上按下 "Go to Details... again" 按钮时，
该应用程序将抛出其中一种典型的 JavaScript 异常——“undefined is not an object”。

```js
class HomeScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
        <Button
          title="Go to Details"
          onPress={() => this.props.navigation.navigate('Details')}
        />
        <DetailsScreen />
      </View>
    );
  }
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/screen-components)

“[Navigation prop 参考](https://reactnavigation.org/docs/navigation-prop.html)”部分会有更多的细节、详细的描述、并提供了更多的信息在 this.props.navigation 的可用属性上。

[【← 下一步】](./next-steps.md)      [【Tab 导航 →】](../howDoIdo/tab-based-navigation.md)
