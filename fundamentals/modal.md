# 开启全屏模式

Dictionary.com 没有提供令人满意的模态定义，
因为它与用户界面有关，
但语义UI描述如下：

>显示一个模态框来暂时阻止与主视图的交互

这听起来很正确。
模式就像一个模态框——它不是您的主要导航流里的一部分——它通常有不同的转换，
不同的方式来销毁它，
并且专注于特定的内容或交互。

### 创建一个模态堆栈

```js
class HomeScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    const params = navigation.state.params || {};

    return {
      headerLeft: (
        <Button
          onPress={() => navigation.navigate('MyModal')}
          title="Info"
          color="#fff"
        />
      ),
      /* the rest of this config is unchanged */
    };
  };

  /* render function, etc */
}

class ModalScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text style={{ fontSize: 30 }}>This is a modal!</Text>
        <Button
          onPress={() => this.props.navigation.goBack()}
          title="Dismiss"
        />
      </View>
    );
  }
}

const MainStack = StackNavigator(
  {
    Home: {
      screen: HomeScreen,
    },
    Details: {
      screen: DetailsScreen,
    },
  },
  {
    /* Same configuration as before */
  }
);

const RootStack = StackNavigator(
  {
    Main: {
      screen: MainStack,
    },
    MyModal: {
      screen: ModalScreen,
    },
  },
  {
    mode: 'modal',
    headerMode: 'none',
  }
);
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/full-screen-modal)

一些需要注意的点：
* 我们知道，该 StackNavigator 函数返回一个 React 组件（记得我们在 App 组件中的 <RootStack /> 的渲染）。这个相同的组件可以用作屏幕组件！通过这样做的时候，我们把 StackNavigator 嵌入在了另一个 StackNavigator 内部。在这种情况下，这对我们很有用，因为我们希望为 modal 使用不同的转换样式，并且我们希望在整个堆栈中禁用标题栏。在将来这将是极其重要的，因为对于标签导航，可能每个标签可能会有自己的堆栈！直观地说这可能是你所期望的：当你在标签A上并切换到标签B时，你会希望标签A在你继续使用标签B时保持其导航状态。查看[这个图表](https://reactnavigation.org/docs/assets/modal/tree.png)来看到这个结构导航的例子。
* StackNavigator 的 modal 可以是 card（默认）或 modal 之一。该 modal 行为在 iOS 上从底部滑动屏幕，并允许用户从顶部向下缩小以关闭该屏幕。该 modal 配置对 Android 没有影响，因为全屏模式在平台上没有任何不同的转场动画。
* 当我们调用 navigate 导航时，我们不必指定除我们想要导航到的路由以外的任何内容。没有必要限定它属于哪个堆栈（任意命名的'root'或'main'堆栈）——React Navigation 尝试在最近的导航栈上查找路由，并在那里执行操作。为了想象出这一点，再看一下[这张图](https://reactnavigation.org/docs/assets/modal/tree.png)，想象一下从 HomeScreen 流式地导航到 MainStack，我们知道 MainStack 无法处理路由 MyModal，然后它将流向 RootStack，这里可以处理该路由。

### 概要

* 要更改 StackNavigator 的某个转场动画类型，您可以使用 mode configuration。设置 modal 时，所有屏幕都从下到上进行转场动画，而不是从右到左。这适用于整个 StackNavigator，所以为了在其他屏幕上使用从右到左的过渡，我们使用默认配置添加另一个导航堆栈。
* this.props.navigation.navigate 遍历 navigator tree 来找到一个可以处理 导航动作的 navigator。
* [至此，我们已经建立了完整的资源](https://snack.expo.io/@react-navigation/full-screen-modal)

[【← 头部按钮】](./header-buttons.md)      [【下一步 →】](./next-steps.md)

