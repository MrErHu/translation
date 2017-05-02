# Configuring the Header

Header仅仅是`StackNavigator`的可选项。

在之前的例子中，我们创建了一个`StackNavigator`在App中显示了不同的界面(screens)。

当我们导航到chat界面时，可以通过将参数传递给`navigate`函数来给新的路由指定参数:

```js
this.props.navigation.navigate('Chat', { user:  'Lucy' });
```

在chat界面可以访问到`user`参数:

```js
class ChatScreen extends React.Component {
  render() {
    const { params } = this.props.navigation.state;
    return <Text>Chat with {params.user}</Text>;
  }
}
```

### 设置Header标题

接下来，Header的标题可以使用界面参数来进行配置:

```js
class ChatScreen extends React.Component {
  static navigationOptions = ({ navigation }) => ({
    title: `Chat with ${navigation.state.params.user}`,
  });
  ...
}
```

```phone-example
basic-header
```


### 添加右侧按钮

接着我们添加一个[`navigation的header选项`](https://reactnavigation.org/docs/navigators/navigation-options#Stack-Navigation-Options)，使得我们可以添加一个自定义的右侧按钮:

```js
static navigationOptions = {
  headerRight: <Button title="Info" />,
  ...
```

```phone-example
header-button
```

`navigation`选项可以通过[`navigation`属性](https://reactnavigation.org/docs/navigators/navigation-prop)定义。让我们基于路由参数来渲染一个不同的按钮，并设置该按钮点击是调用`navigation.setParams`方法。


```js
static navigationOptions = ({ navigation }) => {
  const {state, setParams} = navigation;
  const isInfo = state.params.mode === 'info';
  const {user} = state.params;
  return {
    title: isInfo ? `${user}'s Contact Info` : `Chat with ${state.params.user}`,
    headerRight: (
      <Button
        title={isInfo ? 'Done' : `${user}'s info`}
        onPress={() => setParams({ mode: isInfo ? 'none' : 'info'})}
      />
    ),
  };
};
```

现在，header可以与界面的路由或者状态交互:

```phone-example
header-interaction
```

想要了解关于`header`的其余选项，可以参阅[navigation options document](https://reactnavigation.org/docs/navigators/navigation-options#Stack-Navigation-Options)
