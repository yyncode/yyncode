### 安装React

1. 下载最新版本的nodejs

2. ```
   npx create-react-app my-app
   cd my-app
   npm start
   ```

### React 是什么？

React 是一个声明式，高效且灵活的用于构建用户界面的 JavaScript 库。使用 React 可以将一些简短、独立的代码片段组合成复杂的 UI 界面，这些代码片段被称作“组件”。

React 中拥有多种不同类型的组件。

通常编写组件，子类需要继承React.Component。

```js
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// 用法示例: <ShoppingList name="Mark" />
```

我们通过使用组件来告诉 React 我们希望在屏幕上看到什么。当数据发生改变时，React 会高效地更新并重新渲染我们的组件。

其中，ShoppingList 是一个 **React 组件类**，或者说是一个 **React 组件类型**。一个组件接收一些参数，我们把这些参数叫做 `props`（“props” 是 “properties” 简写），然后通过 `render` 方法返回需要展示在屏幕上的视图的层次结构。

`render` 方法的返回值*描述*了你希望在屏幕上看到的内容。React 根据描述，然后把结果展示出来。更具体地来说，`render` 返回了一个 **React 元素**，这是一种对渲染内容的轻量级描述。大多数的 React 开发者使用了一种名为 “JSX” 的特殊语法，JSX 可以让你更轻松地书写这些结构。语法 `<div />` 会被编译成 `React.createElement('div')`。上述的代码等同于：

```js
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```

在 JSX 中你可以任意使用 JavaScript 表达式，只需要用一个大括号把表达式括起来。每一个 React 元素事实上都是一个 JavaScript 对象，你可以在你的程序中把它保存在变量中或者作为参数传递。

前文中的 `ShoppingList` 组件只会渲染一些内置的 DOM 组件，如`<div />`、`<li />`等。但是你也可以组合和渲染自定义的 React 组件。

例如，你可以通过 `<ShoppingList />` 来表示整个购物清单组件。每个组件都是封装好的，并且可以单独运行，这样你就可以通过组合简单的组件来构建复杂的 UI 界面。

### 通过 Props 传递数据

例如我们写了一个名为Square的组件，在其他组件中这样引用，

```javascript
<Square value={i} />
```

这样就可以把父组件的值传递给子组件，我们可以通过this.props.value在Square组件中获取这个value对应的值

### 给组件添加交互功能

可以通过在 React 组件的构造函数中设置 `this.state` 来初始化 state。`this.state` 应该被视为一个组件的私有属性。我们在 `this.state` 中存储当前每个方格（Square）的值，并且在每次方格被点击的时候改变这个值。

例如：

```javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => console.log('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

> 在 [JavaScript class](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) 中，每次你定义其子类的构造函数时，都需要调用 `super` 方法。因此，在所有含有构造函数的的 React 组件中，构造函数必须以 `super(props)` 开头。

之后我们再通过setState方法改变状态值，例如：

```javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button
        className="square"
        onClick={() => this.setState({value: 'X'})}
      >
        {this.state.value}
      </button>
    );
  }
}
```



### 状态提升

当你遇到需要同时获取多个子组件数据，或者两个组件之间需要相互通讯的情况时，需要把子组件的 state 数据提升至其共同的父组件当中保存。之后父组件可以通过 props 将状态数据传递到子组件当中。这样应用当中所有组件的状态数据就能够更方便地同步共享了。

字面意义来讲，就是我们在父组件中设置state值，然后通过this.state.xxx赋值到子模块引用中，然后在子模块中通过this.props.xxx获取值。这样子模块就不需要拥有state值了，这些提升到了父模块。

### 为什么不可变性在 React 中非常重要

一般来说，有两种改变数据的方式。第一种方式是直接*修改*变量的值，第二种方式是使用新的一份数据替换旧数据。

不直接修改（或改变底层数据）这种方式和前一种方式的结果是一样的，这种方式有以下几点好处：

* 简化复杂的功能
  不可变性使得复杂的特性更容易实现。

* 跟踪数据的改变

  如果直接修改数据，那么就很难跟踪到数据的改变。跟踪数据的改变需要可变对象可以与改变之前的版本进行对比，这样整个对象树都需要被遍历一次。

  跟踪不可变数据的变化相对来说就容易多了。如果发现对象变成了一个新对象，那么我们就可以说对象发生改变了。

* 确定在 React 中何时重新渲染
  不可变性最主要的优势在于它可以帮助我们在 React 中创建 *pure components*。我们可以很轻松的确定不可变数据是否发生了改变，从而确定何时对组件进行重新渲染。

### 函数组件

如果你想写的组件只包含一个 `render` 方法，并且不包含 state，那么使用**函数组件**就会更简单。我们不需要定义一个继承于 `React.Component` 的类，我们可以定义一个函数，这个函数接收 `props` 作为参数，然后返回需要渲染的元素。函数组件写起来并不像 class 组件那么繁琐，很多组件都可以使用函数组件来写。

例如：

```javascript
class Square extends React.Component {
    render() {
        return (
            <button
                className="square"
                onClick={() => this.props.onClick()}>
                {this.props.value}
            </button>
        );
    }
}
```

我们可以改造成：

```javascript
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

我们可以看到，`this.props` 都替换成了 `props`。

当我们把 Square 修改成函数组件时，我们同时也把 `onClick={() => this.props.onClick()}` 改成了更短的 `onClick={props.onClick}`（注意两侧*都*没有括号，不删掉括号，浏览器控制台会报错）。

### 选择一个 key

当我们需要渲染一个列表的时候，React 会存储这个列表每一项的相关信息。当我们要更新这个列表时，React 需要确定哪些项发生了改变。我们有可能增加、删除、重新排序或者更新列表项。

例如：

```javascript
<li>Alexa: 7 tasks left</li>
<li>Ben: 5 tasks left</li>
```

把上面这段代码转变为：

```javascript
<li>Ben: 9 tasks left</li>
<li>Claudia: 8 tasks left</li>
<li>Alexa: 5 tasks left</li>
```

除了数字发生了改变之外，阅读这段代码的人也许会认为我们把 Alexa 和 Ben 的顺序交换了位置，然后把 Claudia 插入到 Alexa 和 Ben 之间。然而，React 是电脑程序，它并不知道我们想要什么。因为 React 无法得知我们人类的意图，所以我们需要给每一个列表项一个确定的 *key* 属性，它可以用来区分不同的列表项和他们的同级兄弟列表项。你可以使用字符串，比如 `alexa`, `ben`, `claudia`。如果我们使用从数据库里获取的数据，那么 Alexa、Ben 和 Claudia 的数据库 ID 就可以作为 key 来使用。

```javascript
<li key={user.id}>{user.name}: {user.taskCount} tasks left</li>
```

每当一个列表重新渲染时，React 会根据每一项列表元素的 key 来检索上一次渲染时与每个 key 所匹配的列表项。如果 React 发现当前的列表有一个之前不存在的 key，那么就会创建出一个新的组件。如果 React 发现和之前对比少了一个 key，那么就会销毁之前对应的组件。如果一个组件的 key 发生了变化，这个组件会被销毁，然后使用新的 state 重新创建一份。

`key` 是 React 中一个特殊的保留属性（还有一个是 `ref`，拥有更高级的特性）。当 React 元素被创建出来的时候，React 会提取出 `key` 属性，然后把 key 直接存储在返回的元素上。虽然 `key` 看起来好像是 `props` 中的一个，但是你不能通过 `this.props.key` 来获取 `key`。React 会通过 `key` 来自动判断哪些组件需要更新。组件是不能访问到它的 `key` 的。

**我们强烈推荐，每次只要你构建动态列表的时候，都要指定一个合适的 key。**如果你没有找到一个合适的 key，那么你就需要考虑重新整理你的数据结构了，这样才能有合适的 key。

如果你没有指定任何 key，React 会发出警告，并且会把数组的索引当作默认的 key。但是如果想要对列表进行重新排序、新增、删除操作时，把数组索引作为 key 是有问题的。显式地使用 `key={i}` 来指定 key 确实会消除警告，但是仍然和数组索引存在同样的问题，所以大多数情况下最好不要这么做。

组件的 key 值并不需要在全局都保证唯一，只需要在当前的同一级元素之前保证唯一即可。

