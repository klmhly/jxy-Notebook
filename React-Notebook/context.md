createContext（）

作用：爷孙（可深层孙）组件之间传值





使用方法：

1. 首先在上层组件使用createContext api 创造一个对象,此对象包含一个【提供方】和【订阅方】

   ```javascript
   const {Provider, Consumer} = React.createContext('default')
   ```

   

2. 在上层组件用Provider 传值

   ```javascript
   <Provider value={1233} >{this.props.children}</Provider>
   ```

   

3. 在下层组件用Consumer使用值

   Consumer 里面是一个函数， 可穿Provider定义的值

   ```javascript
   class Child2 extends React.Component{
       render() {
           return <Consumer>{value => <p>{value}</p>}</Consumer>
       }
   }
   ```

   



源码：

返回一个context对象，

```javascript
const context: ReactContext<T> = {
    $$typeof: REACT_CONTEXT_TYPE,
 
    _currentValue: defaultValue,
    _currentValue2: defaultValue,
   
    // 这个是外部使用的
    Provider: (null: any),
    Consumer: (null: any),
  
  
  
    context.Provider = {
      $$typeof: REACT_PROVIDER_TYPE,
      _context: context,
    };
  	context.Consumer = context;   // Consumer本身就指向这个对象
  };

```

