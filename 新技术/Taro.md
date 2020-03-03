特点

- 多端适配**：从一个端到多个端
- **开发体验**：如支持 React Hooks、CSS Modules、Mobx 等
- **社区共建**：如 [Taro 论坛](https://taro-club.jd.com/)、[Taro物料市场](https://taro-ext.jd.com/) 等平台，以及后面发布的 [社区共建计划](https://taro-club.jd.com/topic/680/taro-邀你加入社区共建-社区共建倡议书)



小程序开发：

微信小程序主要分为 **逻辑层** 和 **视图层**，以及在他们之下的**原生部分**。逻辑层主要负责 JS 运行，视图层主要负责页面的渲染，它们之间主要通过 `Event` 和 `Data` 进行通信，同时通过 `JSBridge` 调用原生的 API。这也是以微信小程序为首的大多数小程序的架构。

![image](https://img12.360buyimg.com/ling/jfs/t1/107177/10/2710/57899/5e09b4d0E0f946516/6f1ce9685dbcb7a8.jpg)



前端关注角度：

![image](https://img13.360buyimg.com/ling/jfs/t1/95252/26/8821/23351/5e09b555Eda94235f/f3c96e14c4c5c218.jpg)

也就是说，只需要在逻辑层调用对应的 `App()/Page()` 方法，且在方法里面处理 data、提供生命周期/事件函数等，同时在视图层提供对应的模版及样式供渲染就能运行小程序了。这也是大多数小程序开发框架重点考虑和处理的部分



Taro 当前架构：

1. 编译时 ： 编译时主要是将 Taro 代码通过 [Babel](https://babeljs.io/) 转换成 小程序的代码，如：`JS`、`WXML`、`WXSS`、`JSON`。
2. 运行时：进行一些：生命周期、事件、data 等部分的处理和对接

编译时最复杂的是JSX编译， **穷举** 的方式对 JSX 可能的写法进行了一一适配，这一部分工作量很大，**实际上 Taro 有大量的 Commit 都是为了更完善的支持 JSX 的各种写法**。

![image](https://img11.360buyimg.com/ling/jfs/t1/105147/3/8169/18536/5e09b73cEf890be4f/a431388d25b2842d.jpg)

运行时的核心： React 的核心 render 方法 没有了。同时代码里增加了 `BaseComponent` 和 `createComponent` ,它们是 Taro 运行时的核心



Taro当前架构特点：

- **重编译时，轻运行时**：这从两边代码行数的对比就可见一斑。
- 编译后代码与 React 无关：Taro 只是在开发时遵循了 React 的语法。
- 直接使用 Babel 进行编译：这也导致当前 Taro 在工程化和插件方面的羸弱。



本质思考：

我们站在浏览器的角度来思考**前端的本质**：无论开发这是用的是什么框架，React 也好，Vue 也罢，最终代码经过运行之后都是调用了浏览器的那几个 BOM/DOM 的 API ，如：`createElement`、`appendChild`、`removeChild` 等。

![image](https://img10.360buyimg.com/ling/jfs/t1/91591/19/8966/30891/5e09f712E6e7f1df8/c3cbbead85810461.jpg)