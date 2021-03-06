# [react][reactjs]

## 声明式

  React 可以非常轻松地创建用户交互界面。为你应用的每一个状态设计简洁的视图，在数据改变时 React 也可以高效地更新渲染界面。
  以声明式编写UI，可以让你的代码更加可靠，且方便调试。
  
## 组件化

  创建好拥有各自状态的组件，再由组件构成更加复杂的界面。
  无需再用模版代码，通过使用JavaScript编写的组件你可以更好地传递数据，将应用状态和DOM拆分开来。
  
## 一次学习，随处编写

  无论你现在正在使用什么技术栈，你都可以随时引入 React 开发新特性。
  React 也可以用作开发原生应用的框架 React Native.
  
### 组件

  React 组件使用一个名为 `render()` 的方法， 接收数据作为输入，输出页面中对应展示的内容。 
  下面这个示例中类似XML的写法被称为JSX. 输入的数据通过 `this.props` 传入 `render()` 方法。

  **使用 React 的时候也可以不使用 JSX 语法** 你可以在 Babel REPL 查看 JSX 是如何被渲染成原生 JavaScript 代码的。

```
class HelloMessage extends React.Component {
  render() {
    return (
      <div>
        Hello {this.props.name}
      </div>
    );
  }
}

ReactDOM.render(
  <HelloMessage name="Taylor" />,
  mountNode
);

Result
Hello Taylor
```

### 有状态组件

  除了使用外部传入的数据以外 (通过 `this.props` 访问传入数据), 组件还可以拥有其内部的状态数据 (通过 `this.state` 访问状态数据)。 
  当组件的状态数据改变时， 组件会调用 `render()` 方法重新渲染。

```
class Timer extends React.Component {
  constructor(props) {
    super(props);
    this.state = { seconds: 0 };
  }

  tick() {
    this.setState(prevState => ({
      seconds: prevState.seconds + 1
    }));
  }

  componentDidMount() {
    this.interval = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.interval);
  }

  render() {
    return (
      <div>
        Seconds: {this.state.seconds}
      </div>
    );
  }
}

ReactDOM.render(<Timer />, mountNode);

Result
Seconds: 1527
```
### 应用

使用 `props` 和 `state`, 我们可以创建一个简易的 `Todo` 应用。 
下面这个示例中，我们使用 `state` 来保存现有的待办事项列表及用户的输入。 
与此同时，我们也使用了内联的方法添加了事件处理函数，它们将通过事件代理被收集和调用。


```
class TodoApp extends React.Component {
  constructor(props) {
    super(props);
    this.state = { items: [], text: '' };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  render() {
    return (
      <div>
        <h3>TODO</h3>
        <TodoList items={this.state.items} />
        <form onSubmit={this.handleSubmit}>
          <input
            onChange={this.handleChange}
            value={this.state.text}
          />
          <button>
            Add #{this.state.items.length + 1}
          </button>
        </form>
      </div>
    );
  }

  handleChange(e) {
    this.setState({ text: e.target.value });
  }

  handleSubmit(e) {
    e.preventDefault();
    if (!this.state.text.length) {
      return;
    }
    const newItem = {
      text: this.state.text,
      id: Date.now()
    };
    this.setState(prevState => ({
      items: prevState.items.concat(newItem),
      text: ''
    }));
  }
}

class TodoList extends React.Component {
  render() {
    return (
      <ul>
        {this.props.items.map(item => (
          <li key={item.id}>{item.text}</li>
        ))}
      </ul>
    );
  }
}

ReactDOM.render(<TodoApp />, mountNode);

Result
TODO
```

### 在组件中使用第三方库

React 的使用非常灵活，并且提供了可以调用其他第三方框架或库的接口。
下面这个示例就使用了一个用来渲染 `markdown` 语法，名为 `remarkable` 的库。
它可以实时转换渲染 `<textarea>` 里的内容。

```
class MarkdownEditor extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = { value: 'Type some *markdown* here!' };
  }

  handleChange(e) {
    this.setState({ value: e.target.value });
  }

  getRawMarkup() {
    const md = new Remarkable();
    return { __html: md.render(this.state.value) };
  }

  render() {
    return (
      <div className="MarkdownEditor">
        <h3>Input</h3>
        <textarea
          onChange={this.handleChange}
          defaultValue={this.state.value}
        />
        <h3>Output</h3>
        <div
          className="content"
          dangerouslySetInnerHTML={this.getRawMarkup()}
        />
      </div>
    );
  }
}

ReactDOM.render(<MarkdownEditor />, mountNode);

Result
Input
Type some *markdown* here!
Output

Type some markdown here!
```

[阮一峰 React 入门实例教程][ruanyifeng]

[React 中文文档 - 用于构建用户界面的 JavaScript 库][reactjscn]

GitHub

[duxianwei520/react][sample]

  一个`react` + `redux` + `webpack` + `ES6` + `antd`的SPA的后台管理底层框架

[antd] 
ant-design

ant A UI Design Language 
一套企业级的 UI 设计语言和 React 实现。

一个服务于企业级产品的设计体系，基于『确定』和『自然』的设计价值观和模块化的解决方案，让设计者专注于更好的用户体验。

蚂蚁金融服务集团

[ant-design/ant-design-pro][antdesignpro]
开箱即用的中台前端/设计解决方案。
特性:

- 优雅美观：基于 Ant Design 体系精心设计
- 常见设计模式：提炼自中后台应用的典型页面和场景
- 最新技术栈：使用 React/dva/antd 等前端前沿技术开发
- 响应式：针对不同屏幕大小设计
- 主题：可配置的主题满足多样化的品牌诉求
- 国际化：内建业界通用的国际化方案
- 最佳实践：良好的工程实践助您持续产出高质量代码
- Mock 数据：实用的本地数据调试方案
- UI 测试：自动化测试保障前端产品质量

模板

- Dashboard
  - 分析页
  - 监控页
  - 工作台
- 表单页
  - 基础表单页
  - 分步表单页
  - 高级表单页
- 列表页
  - 查询表格
  - 标准列表
  - 卡片列表
  - 搜索列表（项目/应用/文章）
- 详情页
  - 基础详情页
  - 高级详情页
- 结果
  - 成功页
  - 失败页
- 异常
  - 403 无权限
  - 404 找不到
  - 500 服务器出错
- 帐户
  - 登录
  - 注册
  - 注册成功

使用

```
$ git clone https://github.com/ant-design/ant-design-pro.git --depth=1
$ cd ant-design-pro
$ npm install
$ npm start         # 访问 http://localhost:8000
```

也可以使用集成化的 ant-design-pro-cli 工具。

```
$ npm install ant-design-pro-cli -g
$ mkdir pro-demo && cd pro-demo
$ pro new
```
更多信息请参考 使用文档。



[reactjs]:https://reactjs.org/
[reactgithub]:https://github.com/facebook/react/
[reactjscn]:https://www.reactjscn.com/
[ruanyifeng]:http://www.ruanyifeng.com/blog/2015/03/react.html
[sample]:https://github.com/duxianwei520/react
[antdesignpro]:https://github.com/ant-design/ant-design-pro












