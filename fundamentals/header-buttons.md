# 头部按钮

现在我们知道如何自定义标题的外观了，
让我们把它们变得可以反馈！
这也许看上去有些刺激，
让我们使它们能够以我们指定的方式回应我们的触摸事件。

### 向标题添加一个按钮

最常见的与标题交互的方式是点击标题左侧或右侧的按钮。
让我们在标题的右侧添加一个按钮（根据手指和手机的大小，这是一个在您的整个屏幕上最难触摸到的地方之一，也是通常用于放置按钮的位置）。

```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    headerTitle: <LogoTitle />,
    headerRight: (
      <Button
        onPress={() => alert('This is a button!')}
        title="Info"
        color="#fff"
      />
    ),
  };
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/simple-header-button)

navigationOptions 中的 this 不是 HomeScreen 的实例，
这样你就不能调用 setState 或它的任何实例方法。
这非常重要，
因为期望头部的按钮与头部所属的屏幕进行交互是非常常见的。
所以，
接下来我们将看看如何做到这一点。

这里有一个社区开发包，
专用于渲染标题中的按钮并提供了正确的样式。
[react-navigation-header-buttons](https://github.com/vonovak/react-navigation-header-buttons)。

### 标题与屏幕组件的交互

使用 params 是最常用的模式去实现在头部按钮中使用屏幕组件实例中的方法。
我们将用一个经典例子来证明这一点，
即计数器。

```js
class HomeScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    const params = navigation.state.params || {};

    return {
      headerTitle: <LogoTitle />,
      headerRight: (
        <Button onPress={params.increaseCount} title="+1" color="#fff" />
      ),
    };
  };

  componentWillMount() {
    this.props.navigation.setParams({ increaseCount: this._increaseCount });
  }

  state = {
    count: 0,
  };

  _increaseCount = () => {
    this.setState({ count: this.state.count + 1 });
  };

  /* later in the render function we display the count */
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/header-interacting-with-component-instance)

>React Navigation 不能保证你的屏幕组件会在屏幕的标题被渲染之前开始装载，
并且由于 increaseCount 的 param 在 componentWillMount 中设置的，
我们在 navigationOptions 中可能还不能使用，
因而我们在获取 params 时包含 || {}（可能没有）。
我们知道这是一个笨拙的API，
我们计划改进它！

>作为 setParams 的替代方案，
您可以使用状态管理库（如 Redux 或 MobX）并在标题和屏幕之间进行通信，
方法与使用两个不同的组件通信相同。

### 自定义后退按钮

StackNavigator 为不同平台提供了默认设置的后退按钮。
在iOS上该按钮旁边有一个标签，
当标题存在可用空间时显示前一屏幕的标题，
否则显示“返回”。

您可以使用 headerBackTitle 和 headerTruncatedBackTitle 更改标签的行为（[阅读更多](https://reactnavigation.org/docs/stack-navigator.html#headerbacktitle)）。

可以使用 headerBackImage 自定义后退按钮图标。

### 覆盖后退按钮

后退按钮会在 StackNavigator 中自动被渲染，
无论何时它能让我们的用户回到前一屏幕——换句话说，
只要有在堆栈超过一个屏幕时后退按钮将被渲染。

一般来说，
这是你所想要的。
但是在某些情况下，
您可能希望通过上述选项自定义后退按钮，
在这种情况下，您可以指定一个 headerLeft，
就像处理 headerRight 一样，
并完全覆盖后退按钮。

### 概要

* 您可以通过 navigationOptions 中的 headerLeft 和 headerRight 属性设置标题中的按钮。
* 后退按钮是完全可定制的 headerLeft，但如果你只是想改变标题或图片，还有其他navigationOptions prop——headerBackTitle、headerTruncatedBackTitle 和 headerBackImage。
* [至此，我们已经建立了完整的资源](https://snack.expo.io/@react-navigation/header-interacting-with-component-instance)

[【← 配置标题栏】](./headers.md)      [【全屏模式 →】](./modal.md)