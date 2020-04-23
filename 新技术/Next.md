1. 模块热替换

   修改代码错误，无需刷新浏览器，页面会立即出现

2. 页面之间跳转

   ```javascript
   <Link  href='/about'><a>链接</a></Link>
   ```

   这是客户端导航。操作在浏览器中进行，而不向服务器发出请求。 您可以通过打开浏览器的网络请求检查器（network request inspector）来验证这一点。

3. `/pages` and `/public` 是特定意义的文件夹

4. 渲染子组件的方式

   1，{props.children}

   2，HOC

   3，页面内容作为prop