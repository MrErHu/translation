# Hello Mobile Navigation

让我们使用React Navigation在iOS和Android平台下创建一个简单的聊天App吧。

## 设置和安装

首先，确保你[安装了React Native](http://facebook.github.io/react-native/docs/getting-started.html)。接下来，创建一个工程并添加`react-navigation`。

```sh
# Create a new React Native App
react-native init SimpleApp
cd SimpleApp

# Install the latest version of react-navigation from npm
npm install --save react-navigation

# Run the new app
react-native run-android # or:
react-native run-ios
```

确保你在iOS 或者 Android平台下能看到空的App:

```phone-example
bare-project
```

我们想要在iOS和Android平台下共享代码，所以我们删除`index.ios.js`和`index.android.js`的内容，并替换为都`import './App';`

现在为我们为App的实现创建新的文件:`App.js`。

## 安装Stack Navigator

对于我们的App，我们想要使用`StackNavigator`,因为我们需要一个概念上类似'栈'的导航栏，我们可以将每一个新的界面(screen)置于栈的顶部，并通过将界面(screen)从栈中移除来返回。让我们从首屏开始:

```js
import React from 'react';
import {
  AppRegistry,
  Text,
} from 'react-native';
import { StackNavigator } from 'react-navigation';

class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    return <Text>Hello, Navigation!</Text>;
  }
}

const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
});

AppRegistry.registerComponent('SimpleApp', () => SimpleApp);
```

界面的`title`配置在[静态的`navigationOptions`](https://reactnavigation.org/docs/navigators/navigation-options),`navigationOptions`中的很多选项(options)可以用来配置在导航栏中的界面的展现。

现在，在iPhone和Android平台上都会展示相同的界面:

```phone-example
first-screen
```

## 增加新的界面

在`App.js`文件中，添加新的界面:`ChatScreen`

```js
class ChatScreen extends React.Component {
  static navigationOptions = {
    title: 'Chat with Lucy',
  };
  render() {
    return (
      <View>
        <Text>Chat with Lucy</Text>
      </View>
    );
  }
}
```

我们可以给`HomeScreen`组件添加一个按钮，通过路由名(`routeName`):`Chat`,使其链接到`ChatScreen`

```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <View>
        <Text>Hello, Chat App!</Text>
        <Button
          onPress={() => navigate('Chat')}
          title="Chat with Lucy"
        />
      </View>
    );
  }
}
```

我们通过使用['navigation'的属性](https://reactnavigation.org/docs/navigators/navigation-prop):`navigate`，使得我们进入了`ChatScreen`界面。但是只有在`StackNavigator`中添加了如下代码才会生效:

```js
const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
  Chat: { screen: ChatScreen },
});
```

现在我们可以导航进入新的界面，并返回:

```phone-example
first-navigation
```

## 参数传递

为`ChatScreen`界面写死name并不理想。相反，如果我们可以传递需要被渲染的name参数将会更有用，我们可以尝试一下。

在`navigate`函数中除了可以指定目标的路由名(`routeName`)，我们可以传递其他的参数，这些参数将会被一起放入新的路由中。首先，我们修改`HomeScreen`组件向路由中传递`user`参数.

```js
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <View>
        <Text>Hello, Chat App!</Text>
        <Button
          onPress={() => navigate('Chat', { user: 'Lucy' })}
          title="Chat with Lucy"
        />
      </View>
    );
  }
}
```

接着我们修改`ChatScreen`组件来展示传入路由的`user`参数:

```js
class ChatScreen extends React.Component {
  // Nav options can be defined as a function of the screen's props:
  static navigationOptions = ({ navigation }) => ({
    title: `Chat with ${navigation.state.params.user}`,
  });
  render() {
    // The screen's current route is passed in to `props.navigation.state`:
    const { params } = this.props.navigation.state;
    return (
      <View>
        <Text>Chat with {params.user}</Text>
      </View>
    );
  }
}
```

现在当你导航到Chat界面中时，可以看见name参数。尝试改变`HomeScreen`中的`user`参数，看看会发生什么！

```phone-example
first-navigation
```