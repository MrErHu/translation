# 集成Redux

为了在Redux中处理应用程序的导航的状态，你可以给导航器(navigator)传递`navigation`属性。`navigation`属性提供当前状态，以及处理导航器选项的`dispatcher`。

在Redux中，应用的状态由`reducer`定义。通过调用`getStateForAction`，每一个导航器路由都拥有一个`reducer`。下面一个例子简洁地演示了何如在Redux应用中使用导航器(`navigators`)。

```
import { addNavigationHelpers } from 'react-navigation';

const AppNavigator = StackNavigator(AppRouteConfigs);

const initialState = AppNavigator.router.getStateForAction(AppNavigator.router.getActionForPathAndParams('Login'));

const navReducer = (state = initialState, action) => {
  const nextState = AppNavigator.router.getStateForAction(action, state);

  // Simply return the original `state` if `nextState` is null or undefined.
  return nextState || state;
};

const appReducer = combineReducers({
  nav: navReducer,
  ...
});

class App extends React.Component {
  render() {
    return (
      <AppNavigator navigation={addNavigationHelpers({
        dispatch: this.props.dispatch,
        state: this.props.nav,
      })} />
    );
  }
}

const mapStateToProps = (state) => ({
  nav: state.nav
});

const AppWithNavigationState = connect(mapStateToProps)(App);

const store = createStore(appReducer);

class Root extends React.Component {
  render() {
    return (
      <Provider store={store}>
        <AppWithNavigationState />
      </Provider>
    );
  }
}
```

一旦你这么做了，导航栏(navigation)的状态就会被存储在`redux`中，这时你可以通过使用`redux`的`dispatch`来激活导航栏(navigation)操作。

需要牢记的是，当给导航栏(navigation)传递了`navigation`参数，它就放弃了对内部状态的控制。这意味着你有责任保持状态(state)，处理深层次的联系，以及集成返回按钮等。

当你嵌套使用的时候，导航器状态(Navigation state)自动地从一个导航器传递到另一个导航器中。需要注意地是，为了使子导航栏能从父导航器接收状态，应该将其定义成一个`screen`。

将其应用于上述示例，你可以将定义AppNavigator为嵌套包含TabNavigator的组件，如下所示:

```js
const AppNavigator = StackNavigator({
  Home: { screen: MyTabNavigator },
});
```

在这个例子中，一旦将`AppNavigator`连接(`connect`)到Redux，就像`AppWithNavigationState`一样，`MyTabNavigator`将能自动访问到导航栏状态(navigation state)。

## 完整实例

如果你想自己尝试一下，这是一个使用`redux`的[完整实例](https://github.com/react-community/react-navigation/tree/master/examples/ReduxExample)。

## 模拟测试

为了使得react-navigation应用可测试，你需要改变package.json的jest预订参数，例如:

```
"jest": {
  "preset": "react-native",
  "transformIgnorePatterns": [
    "node_modules/(?!(jest-)?react-native|react-navigation)"
  ]
}
```
