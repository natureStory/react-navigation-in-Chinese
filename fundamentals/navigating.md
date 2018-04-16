# 在屏幕之间跳转

在上一节 “[Hello React Navigation](./hello-react-navigation.md)” 中，
我们定义了包含两个路由（Home 和 Details）的 StackNavigator，
但我们没有学习如何让用户从 Home 导航到 Details
（尽管我们已经学会了如何更改代码中的初始路由，
但是迫使我们的用户克隆我们的存储库并在我们的代码中改变路由，
以便看到另一个屏幕可能是人们可以想象的最糟糕的用户体验之一，哈哈）。

如果这是一个web浏览器，我们可以这样写：

```html
<a href="details.html">Go to Details</a>
```

另一种写法：

```html
<a onClick={() => { document.location.href = "details.html"; }}>Go to Details</a>
```

我们的做法类似于后者，
但不是使用全局的 document，
而是使用 navigation prop 的方法在屏幕组件中传递。

### 导航到新的屏幕

```js
import React from 'react';
import { Button, View, Text } from 'react-native';
import { StackNavigator } from 'react-navigation';

class HomeScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
        <Button
          title="Go to Details"
          onPress={() => this.props.navigation.navigate('Details')}
        />
      </View>
    );
  }
}

// ... other code from the previous section
```
👉 [运行此代码](https://snack.expo.io/@react-navigation/our-first-navigate)

我们来分解一下：

* this.props.navigation：navigation prop 被传递给每个屏幕组件（[定义](./glossary-of-terms.md)）StackNavigator（稍后在“[深入理解navigation prop](../API/)”中稍后介绍）。
* navigate('Details')：我们将这个 navigate 函数（在 navigation prop 上命名 - 命名很难！）称为我们即将跳转去的屏幕的路由。

>如果我们this.props.navigation.navigate使用我们尚未定义的路由名称进行跳转，
则 StackNavigator 不会发生任何事情。
换句话说，
我们只能导航到 StackNavigator 中已定义的路由——我们无法导航到任意组件。

所以我们现在有两条路由：1）Home 路由2）Details 路由。
如果我们再次从 Details 屏幕导航到 Details 路由，会发生什么呢？

### 多次导航到路由

```js
class DetailsScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Details Screen</Text>
        <Button
          title="Go to Details... again"
          onPress={() => this.props.navigation.navigate('Details')}
        />
      </View>
    );
  }
}
```
👉 [运行此代码](https://snack.expo.io/@react-navigation/navigating-to-details-again)

如果您运行此代码，
您会注意到，
每次按 “Go to Details... again” 按钮时，
它都会将新的屏幕推到顶部。
这是与我们原来的 document.location.href 相比较所不同的地方，
因为在web浏览器中，
这些不会被视为不同的路由，
也不会将新记录添加到浏览器的历史记录中——StackNavigator 的 navigate 行为更像 web 的 window.history.pushState：每当您调用时 navigate 都会把新的路由推入导航堆栈。

### 回退

StackNavigator 提供的头部组件自动包含了一个可从当前活动页面回退的按钮（如果导航堆栈中只有一个屏幕，则没有任何可返回的内容，因此也没有后退按钮）。

有时你会希望能够以程序的方式触发这种行为，
则可以使用 this.props.navigation.goBack(); 。

```js
class DetailsScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Details Screen</Text>
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

👉 [运行此代码](https://snack.expo.io/@react-navigation/going-back)

>在Android上，
React Navigation 关联到硬件的后退按钮，
并且 goBack() 在用户按下时触发该功能，
因此它的行为与用户期望的相同。

另一个常见的要求是能够返回多个屏幕——例如，
如果您在一个堆栈中有几个深度的屏幕，
并且想要销毁所有屏幕以返回到第一个屏幕。
我们将在“[认证流程](../howDoIdo/auth-flow.md)”中讨论如何做到这一点。

### 概要

* this.props.navigation.navigate('RouteName') 把新的路由推入 StackNavigator。我们可以多次调用它，并且会继续推入路由。
* 标题栏将自动显示后退按钮，但您可以通过编程的方式调用 this.props.navigation.goBack() 返回。在 Android 上，物理后退按钮正常工作。
* navigation prop 可用于所有的屏幕组件（在路由配置中定义为屏幕的组件并作为 React Navigation 的路由去渲染）。
* [至此，我们已经建立了完整的资源](https://snack.expo.io/@react-navigation/going-back)

[【← hello React Navigation】](./hello-react-navigation.md)      [【路由传参 →】](./params.md)