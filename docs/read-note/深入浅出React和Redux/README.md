# 深入浅出React和Redux
## 1、React新的前端思维方定义
### 虚拟DOM
Web前端开发关于性能有一个原则：**尽量减少DOM操作。** 虽然DOM操作也只是一些简单的JavaScript语句，但是DOM操作会引起浏览器对网页进行重新布局，重新绘制，这就是一个比JavaScript语句执行慢很多的过程。
## 2、设计高质量的React组件
### 易于维护组件的设计要素
软件设计的通则，组件的划分要满足高内聚（High Cohesion）和低耦合（Low Coupling）

**高内聚** 指把逻辑紧密相关的内容放在一个组件中。用户界面无外乎内容、交互行为和样式。传统上，内容由HTML表示，交互行放在JavaScript代码文件中，样式放在CSS文件中定义。这虽然满足一个功能模块的需要，却要放在三个不同的文件中，这其实不满足高内聚的原则。React却不是这样的。展示内容的JSX、定义行为的JavaScript代码、甚至定义样式的CSS，都可以放在一个JavaScript文件中，因为它们本来就是为了实现一个目标而存在的。

**低耦合** 指的是不同组件之间的依赖关系要尽量弱化，也就是每个组件要尽量独立。保持整个系统的低耦合度，需要对系统中的功能有充分的认识，然后根据功能点划分模块，让不同的组件去实现不同的功能，这个功夫在开发者身上。

### React组件数据
“差劲的程序员操心代码，优秀的程序员操心数据结构和它们之间的关系。”——Linus Torvalds， Linux创始人

#### 读取prop值 

```js
class ClickCounter extends Component {
  constructor(props) {
    super(props);
    this.onClickButton = this.onClickButton.bind(this)
    this.state = {
      count: props.initValue || 0
    }
  }
}
```
如果一个组件需要定义自己的构造函数，一定要记得在构造函数的第一行通过`super`调用父类也就是`React.Component`的构造函数。如果在构造函数中没有调用`super(props)`那么组件的实例被构造之后，类实例的所有成员函数就无法通过`this.props`访问到父组件传递过来的`props`值。很明显，给`this.props`赋值是`React.Component`构造函数的工作之一。'

### 组件的生命周期
React严格定义了组件的生命周期：
* 装载过程（Mount） 把组件第一次在DOM树中渲染的过程
* 更新过程（Update） 当组件被重新渲染的过程
* 装载过程（Unmount） 组件从DOM中删除的过程

## 3、从Flux到Redux
### Redux
#### Redux的基本准则
* 唯一数据源
* 保持状态可读
* 数据改变只能通过纯函数完成

**唯一数据源**
指应用状态的数据应该只存储在唯一的一个Store上。

**保持状态可读**
不能去直接修改状态，要修改Store状态，必须要通过派发一个action对象完成。

**数据只能通过纯函数完成**
这里的纯函数就是reducer