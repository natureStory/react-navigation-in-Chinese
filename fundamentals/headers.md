# 配置标题栏

现在，
你可能已经厌倦了在屏幕上看到一个空白的灰色条——你已经准备好跃跃欲试了（[flair](https://memegenerator.net/img/images/600x600/14303485/stan-flair-office-space.jpg)）。
那么让我们来看看标题栏的配置吧。

### 设置标题栏标题

屏幕组件可以具有一个静态属性 navigationOptions，
该属性可以是对象或返回配置选项对象的函数。
我们用于标题栏标题设置的是 title，
如以下示例所示。

```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
  };

  /* render function, etc */
}

class DetailsScreen extends React.Component {
  static navigationOptions = {
    title: 'Details',
  };

  /* render function, etc */
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/setting-header-title)

StackNavigator 默认情况下使用平台默认配置，所以在iOS上标题将居中，在Android上它将左对齐。

### 在标题中使用参数

为了在标题栏中使用参数，
我们要把 navigationOptions 写成一个返回配置对象的函数。
我们可能去尝试使用 this.props 来代替 navigationOptions，
但是因为它是组件的静态属性，
this 并不涉及组件的实例，
因此没有可用的 props。
这种情况下，
如果我们把 navigationOptions 写成一个函数，
那么React Navigation 会使用一个对象{ navigation, navigationOptions, screenProps }去调用它——在这种情况下，
我们关心的是 navigation，
这是与传递给屏幕组件的 this.props.navigation 所相同的。
您可能还记得我们可以通过 navigation.state.params 获取 navigation 的参数，
因此我们采用以下方法在标题栏中获取参数。

```js
class DetailsScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    const { params } = navigation.state;
    
    return {
      title: params ? params.otherParam : 'A Nested Details Screen',
    }
  };

  /* render function, etc */
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/using-params-in-title)

传递给 navigationOptions 函数的参数是一个具有以下属性的对象：
* navigation——屏幕的 navigation prop，屏幕的路由在 navigation.state
* screenProps——从导航组件的上层传递而来的 prop
* navigationOptions——如果未提供，将使用默认或先前的配置

在上面例子中我们仅需要 navigation prop，
但在某些情况下您可能想要使用 screenProps 或 navigationOptions。

### 通过 setParams 更新 navigationOpations

经常有需求从加载的活动屏幕组件去更新 navigationOptions 的配置。
我们可以通过 this.props.navigation.setParams 来完成

```
/* Inside of render() */
  <Button
    title="Update the title"
    onPress={() => this.props.navigation.setParams({otherParam: 'Updated!'})}
  />
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/updating-navigationoptions-with-setparams)

### 调整标题样式

要自定义标题栏的样式时，使用三个关键属性：headerStyle，headerTintColor，和headerTitleStyle。
* headerStyle：应用于包裹标题的 View 的样式对象。如果你设置了 backgroundColor，这将是你的标题栏的背景颜色。
* headerTintColor：后退按钮和标题字都使用此属性作为它们的颜色。在下面的例子中，我们将tint颜色设置为white（#fff），所以后退按钮和标题字将变为白色。
* headerTitleStyle：如果我们想自定义 fontFamily，fontWeight 等 Text 为标题样式属性，我们可以用它来做到这一点。

```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
    headerStyle: {
      backgroundColor: '#f4511e',
    },
    headerTintColor: '#fff',
    headerTitleStyle: {
      fontWeight: 'bold',
    },
  };

  /* render function, etc */
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/setting-header-styles)

注意：
1. 在iOS上，状态栏文本和图标是黑色的，而且在深色背景下看起来不太好。我们暂且不讨论它，但您应该确保配置状态栏以符合屏幕颜色，[如状态栏指南中所述](https://reactnavigation.org/docs/status-bar.html)。
2. 我们设置的配置仅适用于 Home 屏幕; 当我们导航到 Details 屏幕时会恢复默认样式。现在让我们来看看如何在屏幕之间共享 navigationOptions 配置。

### 跨屏共享 navigationOptions

通常希望在多个屏幕上以类似的方式配置标题栏。
例如，
您的公司品牌颜色可能为红色，
因此您希望标题背景颜色为红色，
浅色为白色。
顺便地，
这也是我们正在使用的运行示例的颜色，
并且您会注意到当您导航到 DetailsScreen 时颜色会返回到默认值。
如果我们不得不从 HomeScreen 把 navigationOptions 标题栏样式属性复制到 DetailsScreen 以及我们在应用程序中的每个 screen 组件，岂不是很糟糕吗？好在我们不需要。我们可以改为将 configuration 写在 StackNavigator。

```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
    /* No more header config here! */
  };

  /* render function, etc */
}

/* other code... */

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
    /* The header config from HomeScreen is now here */
    navigationOptions: {
      headerStyle: {
        backgroundColor: '#f4511e',
      },
      headerTintColor: '#fff',
      headerTitleStyle: {
        fontWeight: 'bold',
      },
    },
  }
);
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/sharing-header-styles)

现在，
RouteStack 里的任何屏幕组件都有了我们的品牌风格。
当然，
如果有必要，我们如何重写这些配置？

### 重写共享的 navigationOptions

屏幕组件上指定的 navigationOptions 与其先前的 StackNavigator 合并，
并以屏幕组件上的配置项优先。
让我们使用这些知识在 details 屏幕上更改背景和颜色。

```js
class DetailsScreen extends React.Component {
  static navigationOptions = ({ navigation, navigationOptions }) => {
    const { params } = navigation.state;

    return {
      title: params ? params.otherParam : 'A Nested Details Screen',
      /* These values are used instead of the shared configuration! */
      headerStyle: {
        backgroundColor: navigationOptions.headerTintColor,
      },
      headerTintColor: navigationOptions.headerStyle.backgroundColor,
    };
  };

  /* render function, etc */
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/overriding-shared-header-styles)

### 用自定义组件替换标题栏


有时候，
您需要更多的控制权，
而不仅仅是改变标题栏的文本和样式——例如，
您可能想渲染图像来代替标题，
或者将标题变为按钮。
在这些情况下，
您可以使用自己的组件来覆盖标题栏组件。

```js
class LogoTitle extends React.Component {
  render() {
    return (
      <Image
        source={require('./spiro.png')}
        style={{ width: 30, height: 30 }}
      />
    );
  }
}

class HomeScreen extends React.Component {
  static navigationOptions = {
    // headerTitle instead of title
    headerTitle: <LogoTitle />,
  };

  /* render function, etc */
}
```

👉 [运行此代码](https://snack.expo.io/@react-navigation/custom-header-title-component)

>你可能想知道，
为什么当我们提供一个组件时使用 headerTitle 而不是像之前的 title？
原因是 headerTitle 是 StackNavigator 的一个特定属性，
headerTitle 默认为一个显示 title 的文本组件。

### 其他配置
 
您可以在 [StackNavigator参考](https://reactnavigation.org/docs/stack-navigator.html#navigationoptions-used-by-stacknavigator) 中阅读 StackNavigator 可用屏幕的 navigationOptions 的完整列表。

### 概要

* 您可以在屏幕组件的静态属性 navigationOptions 内自定义标题。完整配置列表请参阅[API参考](https://reactnavigation.org/docs/stack-navigator.html#navigationoptions-used-by-stacknavigator)。
* navigationOptions 的静态属性可以是对象或者方法。当它是方法时，他会提供包含 navigation prop、screenProps 和 navigationOptions 的对象(的参数)。
* 初始化时，您可以在 StackNavigator 中配置共享的 navigationOptions。(屏幕组件中的)静态属性优先于此配置。
* [至此，我们已经建立了完整的资源](https://snack.expo.io/@react-navigation/custom-header-title-component)

[【← 路由传参】](./params.md)      [【头部按钮 →】](./header-buttons.md)