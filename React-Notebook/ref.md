

Ref：是为了获取某个节点的实例

源码

路径  react-16.7.0/packages/react/ReactCreateRef.js

核心源码

```javascript
export function createRef(): RefObject {
  const refObject = {
    current: null,
  };
  ...
  return refObject;
}
```



返回一个对象， 这个对象有一个current属性， 这是下面ref的第3种使用方法:

`this.objRef = React.createRef()       // 生成了这个对象   {current: null}`



留一些疑问 ？？



ref的三种使用方式：

1. ref = "string"     (不推荐)  
2. ref = {ele => {this.methodRef = ele}}.   
3. ref = {this.objRef}

```javascript
// 例如： 

constructor () {
  this.objRef = React.createRef()
  // {current: null}
}

componentDidMount(){
  // 可以取到ref关联的节点
  this.refs.p1.textContent = 'abc'
  this.methodRef.textContent = 'method ref get'
  this.objRef.current.textContent = 'obj hhh'
}

<p ref ='p1'>hello world</p>
<p ref = {ele => {this.methodRef = ele}}>hello world</p>
<p ref = {this.objRef}>hello</p>
```





fowardRef 用处 ： ？

源码：

