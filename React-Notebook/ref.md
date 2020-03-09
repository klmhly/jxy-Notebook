

[TOC]

## 一 、概念

Ref：创建一个可以绑定dom元素的对象

forwardRef:  创建一个React**组件**，将其接受的ref属性转发到组件树的另一组件



## 二、源码

#### 1. createRef源码: 

创建一个可以绑定dom元素的对象

```javascript
export function createRef(): RefObject {
  const refObject = {
    current: null,
  };
  ...
  return refObject;
}
```

解析：返回一个对象， 这个对象有一个current属性，例如：在组件中定义一个ref对象： `this.objRef = React.createRef()       // 生成了这个对象   {current: null}`， 可以指向dom元素

#### 2. fowardRef 源码

创建一个React**组件**，将其接受的ref属性转发到组件树的另一组件

```javascript
export default function forwardRef<Props, ElementType: React$ElementType>(
  render: (props: Props, ref: React$Ref<ElementType>) => React$Node,
) {
  ... // 省略dev代码
  return {
    $$typeof: REACT_FORWARD_REF_TYPE,
    render,
  };
}
```

解析： 创建一个React**组件**，将其接受的ref属性转发到组件树的另一组件， 一旦在`Father`组件中，用`JSX`引用了`Child`组件，那么就是`React.createElement(React.forwardRef(Child))`，又包裹了一层，此时的`$$typeof`是`REACT_ELEMENT_TYPE`，`type`是`React.forwardRef(Child)`，`type`里面的`$$typeof`是`REACT_FORWARD_REF_TYPE`



## 三、用途

1. 获取 **当前组件中的某个dom元素** 实例

2. 获取 **子组件** 实例

3. 获取 **子组件里面的dom元素** 实例

   

## 四、用途示例

#### 1. 获取 **当前组件中的某个dom元素** 实例        【ref】

ref = "string"     (不推荐)   // this.refs.strName   绑定元素

ref = {ele => {this.methodRef = ele}}.   

ref = {this.objRef}         //React.createRef()

```javascript
// 例如： 
constructor () {
  this.objRef = React.createRef()
  // {current: null}
}

componentDidMount(){
  // 可以取到ref关联的节点
  this.refs.p1.textContent = 'abc'       // 不被推荐（可能已经被废弃）
  this.methodRef.textContent = 'method ref get'
  this.objRef.current.textContent = 'obj hhh'     //使用了ref api,推荐方式
}

<p ref ='p1'>hello world</p>   // 1
<p ref = {ele => {this.methodRef = ele}}>hello world</p>   //2
<p ref = {this.objRef}>hello</p>     //3
```

#### 2. 获取 **子组件** 实例

#### 3. 获取 **子组件里面的dom元素** 实例

可以在父组件中直接操作子组件的dom元素

> fowardRef参数： 渲染函数 （props, ref) => (<div>12345</div>) 
>
> 返回了包装了原始组件的另一个组件

```javascript
import React from 'react'

const Child = React.forwardRef((props, ref) => (
    <div>
        <button ref={ref}>点我</button>
    </div>
))

class Parent extends React.Component {
    constructor (props) {
        super(props)
        this.ref = React.createRef()
        this.refP = React.createRef()
    }
    componentDidMount(){
        console.log('打印：', this.ref.current)      //打印： <button>点我</button>
    }
    render() {
        return (
            <div>
                <Child ref={this.ref} />           //这个ref 指向子组件里面的button
            </div>   
        )
    }
}

export default Parent
```



## 五、总结

1. 转发refs到DOM组件 
2. 在高阶组件中转发refs






