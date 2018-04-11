# Hello React Navigation

在Web浏览器中，您可以使用 anchor（\<a\>）标签链接到不同的页面。当用户点击一个链接时，URL 被推入到浏览器历史堆栈。当用户按下后退按钮时，浏览器从历史堆栈的顶部推出该记录，因此显示页面现在是先前访问过的页面。React Native 没有使用像Web浏览器那样的内置全局历史堆栈的想法—React Navigation 有着自己的故事。

React Navigation's StackNavigator 为您的应用程序在屏幕之间切换并管理导航历史记录提供了一种途径。如果您的应用只使用一个 StackNavigator，那么它在概念上与web浏览器处理导航状态的方法类似——你的 APP 出入栈你的记录来保持与用户的交互，这会导致用户会看到不同的屏幕。在Web浏览器和React Navigation中工作原理的一个主要区别是，StackNavigator提供了在Android和iOS上在堆栈中的路由之间导航时你所期望的手势和动画。

我们需要从使用 React Navigation 的 StackNavigator 开始。

# 创建一个StackNavigator

StackNavigator 是一个返回 React 组件的函数。它接受一个路由配置对象，并且可选地包含一个选项对象（目前我们可以省略它）。因为该 StackNavigator 函数返回一个 React 组件，所以我们可以把它从 App.js 导出来，从而作为我们应用程序的根组件。

```js
// In App.js in a new project

import React from 'react';
import { View, Text } from 'react-native';
import { StackNavigator } from 'react-navigation';

class HomeScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Home Screen</Text>
      </View>
    );
  }
}

export default StackNavigator({
  Home: {
    screen: HomeScreen,
  },
});
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/hello-world)

如果您运行此代码，您将看到一个屏幕，其中有一个空的导航栏和一个包含您的HomeScreen组件的灰色内容区域。您看到的导航栏和内容区域的样式是默认配置的 StackNavigator，我们将学习如何配置这些样式。

* 路由名称的大小写无关紧要——您可以使用小写字母 home 或大写字母 Home，这取决于您。我们更喜欢大写我们的路由名称。
* 路由唯一必须配置的是屏幕组件。您可以阅读[StackNavigator参考](https://reactnavigation.org/docs/stack-navigator.html)中可用的其他选项的更多信息。

在 React Native 中，从其中导出的组件 App.js 是您的应用程序的入口点（或根组件） - 每个其他组件均继承于它。相比于你直接得到一个被导出的 StackNavigator，通常来说在你的 APP 下的根组件拥有更多的控制权更有用处，所以让我们导出一个只呈现我们 StackNavigator 的组件。

```js
const RootStack = StackNavigator({
  Home: {
    screen: HomeScreen,
  },
});

export default class App extends React.Component {
  render() {
    return <RootStack />;
  }
}
```

# 添加第二个路由

该<RootStack />组件不接受任何 props——所有配置都在 options 参数中指定给该 StackNavigator 方法。我们未配置 options，所以它使用了默认配置。要查看使用该 options 对象的示例，我们将向为 StackNavigator 添加第二个屏幕。

```js
// Other code for HomeScreen here...

class DetailsScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text>Details Screen</Text>
      </View>
    );
  }
}

const RootStack = StackNavigator(
  {
    Home: {
      screen: HomeScreen,
    },
    Details: {
      screen: DetailsScreen,
    },
  },
  {
    initialRouteName: 'Home',
  }
);

// Other code for App component here...
```

现在我们的堆栈有两个路由，一个 Home 路由和一个 Details 路由。Home 路由对应 HomeScreen 组件，Details 路由对应 DetailsScreen 组件。堆栈的初始路由是 Home 路由。自然地，我们会提出疑问："我如何从 Home route 跳转至 Details route？"这将在下一章节中介绍。

# 概要

* React Native 没有像Web浏览器那样的内置 API 用于导航。React Navigation为您提供了iOS和Android通过手势和动画以在屏幕之间切换的功能。
* StackNavigator 是一个接收路由配置对象和选项对象并返回 React 组件的函数。
* 路由配置对象中的键是路由名称，值是该路由的配置。配置中唯一必须的属性是screen（用于路由的组件）。
* [至此，我们已经建立了完整的资源](https://snack.expo.io/@react-navigation/hello-react-navigation)

[【起步 →】](./start.md)      [【跳转 →】](./...)