# Navigators

Navigators 允许你为你的应用定义导航栏结构(navigation structure)。Navigator会按照你所配置的，渲染类似于:头部(headers)和标签栏(tab bars)等常见元素。

原理上，Navigators仅仅只是纯React组件。

## 内置导航栏

`react-navigation`包含以下函数，帮助你创建导航栏:

- [StackNavigator](https://reactnavigation.org/docs/navigators/stack) -在某一时刻渲染界面，并提供界面之间过渡。当打开新的界面，它将被置于栈的顶部。
- [TabNavigator](https://reactnavigation.org/docs/navigators/tab) -渲染标签栏，使得用户可以在不同的界面间切换
- [DrawerNavigator](https://reactnavigation.org/docs/navigators/drawer) - 提供一个抽屉窗口(drawer)，其可以从屏幕左侧滑出。


## Rendering screens with Navigators

The navigators render application screens which are just React components.

To learn how to create screens, read about:
- [Screen `navigation` prop](https://reactnavigation.org/docs/navigators/navigation-prop) to allow the screen to dispatch navigation actions, such as opening another screen
- [Screen `navigationOptions`](https://reactnavigation.org/docs/navigators/navigation-options) to customize how the screen gets presented by the navigator (e.g. header title, tab label)

### 在顶层组件中调用Navigate

万一你想要在声明导航栏的相同层级使用导航栏，你可以使用React的[`ref`](https://facebook.github.io/react/docs/refs-and-the-dom.html#the-ref-callback-attribute)

```js
const AppNavigator = StackNavigator(SomeAppRouteConfigs);

class App extends React.Component {
  someEvent() {
    // call navigate for AppNavigator here:
    this.navigator && this.navigator.dispatch({ type: 'Navigate', routeName, params });
  }
  render() {
    return (
      <AppNavigator ref={nav => { this.navigator = nav; }} />
    );
  }
}
```

需要注意的是，这种解决方案仅可以在顶层的导航栏中使用。

## 导航栏容器(Navigation Containers)

当导航栏的属性不存在时，内置的导航栏将自动为顶层导航栏。它从功能上提供了一个透明的导航栏容器，可以为顶层导航栏提供属性来源。


当渲染其中一个导航栏时，其属性可以可选的。当属性缺失时，容器将管理自己的导航栏状态。它也会处理URL，外部链接，和集成的安卓返回按钮。

为了方便使用，内置的导航器具备该能力，因为在后台需要使用`createNavigationContainer`。通常情况下，导航栏为了起作用接受如下参数:

### `onNavigationStateChange(prevState, newState, action)`

导航器管理的导航状态改变时，函数每次都会被调用。函数接受之前的状态，导航栏新的状态和导致状态变化的行为(actions)。默认地，每次状态变化都会在控制台打印。

### `uriPrefix`

应用程序可能会处理URI的前缀。这可能会被用来处理从[深层链接](https://reactnavigation.org/docs/guides/linking)中提取路径并传入路由中。

